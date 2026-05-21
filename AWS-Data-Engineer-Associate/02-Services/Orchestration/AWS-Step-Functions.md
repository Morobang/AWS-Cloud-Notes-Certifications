# AWS Step Functions (Orchestration Reference)

> This is the Orchestration-category reference for Step Functions. See also [Processing/AWS-Step-Functions.md](../Processing/AWS-Step-Functions.md) for the detailed service deep dive.

---

## Quick Reference

AWS Step Functions is the AWS-native serverless orchestration service. For data pipelines that are AWS-centric and don't require Airflow compatibility, Step Functions is the default choice.

---

## Data Pipeline Orchestration Patterns

### ETL Pipeline with Fan-Out

Process multiple data sources in parallel, then combine:

```
EventBridge (trigger)
    ↓
Step Functions: Parallel state
  ├── Branch A: Crawl + ETL for sales data
  ├── Branch B: Crawl + ETL for inventory data
  └── Branch C: Crawl + ETL for customer data
    ↓ (all branches complete)
Combine and load to Redshift
    ↓
Notify team via SNS
```

The `Parallel` state waits for all branches to complete before moving on.

### Map State for File Processing

```json
{
  "ProcessAllFiles": {
    "Type": "Map",
    "ItemsPath": "$.file_list",
    "MaxConcurrency": 20,
    "Iterator": {
      "StartAt": "ProcessOneFile",
      "States": {
        "ProcessOneFile": {
          "Type": "Task",
          "Resource": "arn:aws:states:::lambda:invoke",
          "Parameters": {
            "FunctionName": "ProcessFile",
            "Payload.$": "$"
          },
          "End": true
        }
      }
    },
    "Next": "Aggregate"
  }
}
```

### Human Approval Workflow

Pause a pipeline and wait for human approval before proceeding (e.g., before loading data to production):

```
Step 1: Run data quality checks
    ↓
Step 2: Wait for Approval (Task with .waitForTaskToken)
  → Send SNS notification with taskToken to approver
  → Approver reviews data quality report
  → Approver calls SendTaskSuccess/SendTaskFailure API
    ↓
Step 3 (if approved): Load to production
Step 3 (if rejected): Quarantine and alert
```

---

## Step Functions vs MWAA Summary

| Factor | Step Functions | MWAA |
|---|---|---|
| Existing Airflow codebase | No | Yes |
| AWS-native integrations | Native (no code) | Via operators |
| Multi-service pipeline | Yes | Yes |
| Open-source portability | No | Yes |
| Cost for idle pipelines | Free (no executions) | Always-on cost |
| Duration | Up to 1 year | No limit |
| Best for | New AWS projects, simple-medium complexity | Existing Airflow, complex dependency graphs |
