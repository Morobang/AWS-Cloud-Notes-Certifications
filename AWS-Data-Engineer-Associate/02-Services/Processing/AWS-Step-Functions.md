# AWS Step Functions

**Category**: Serverless Orchestration  
**Exam weight**: High — the primary way to orchestrate multi-step data pipelines in AWS

---

## What Is It?

AWS Step Functions is a serverless visual workflow service that orchestrates AWS services into multi-step pipelines. You define the workflow in Amazon States Language (ASL, a JSON superset) and Step Functions manages execution, retry logic, error handling, and state transitions.

---

## Core Concepts

### State Types

| State | What it does |
|---|---|
| `Task` | Do work — call Lambda, Glue, EMR, ECS, Batch, SQS, SNS, DynamoDB, or any AWS API |
| `Choice` | Branch based on a condition (if/else logic) |
| `Wait` | Pause execution for N seconds or until a timestamp |
| `Parallel` | Run multiple branches concurrently |
| `Map` | Run the same workflow for each item in an array |
| `Pass` | Pass input to output unchanged (useful for testing/debugging) |
| `Succeed` | End the workflow successfully |
| `Fail` | End the workflow with an error |

### Execution Model

**State machine**: The complete workflow definition.

**Execution**: One run of the state machine with a specific input. Each execution is independent.

```json
{
  "Comment": "Daily ETL pipeline",
  "StartAt": "StartGlueCrawler",
  "States": {
    "StartGlueCrawler": {
      "Type": "Task",
      "Resource": "arn:aws:states:::glue:startCrawler.sync",
      "Parameters": {
        "Name": "sales-raw-crawler"
      },
      "Next": "RunGlueJob"
    },
    "RunGlueJob": {
      "Type": "Task",
      "Resource": "arn:aws:states:::glue:startJobRun.sync",
      "Parameters": {
        "JobName": "transform-sales",
        "Arguments": {
          "--date.$": "$.processing_date"
        }
      },
      "Retry": [
        {
          "ErrorEquals": ["Glue.AWSGlueException"],
          "IntervalSeconds": 60,
          "MaxAttempts": 2,
          "BackoffRate": 2
        }
      ],
      "Catch": [
        {
          "ErrorEquals": ["States.ALL"],
          "Next": "NotifyFailure"
        }
      ],
      "Next": "CheckJobResult"
    },
    "NotifyFailure": {
      "Type": "Task",
      "Resource": "arn:aws:states:::sns:publish",
      "Parameters": {
        "TopicArn": "arn:aws:sns:us-east-1:123456789:pipeline-alerts",
        "Message": "ETL job failed"
      },
      "End": true
    }
  }
}
```

---

## Workflow Types

| Type | Duration | Execution | Audit history | Cost | Use case |
|---|---|---|---|---|---|
| **Standard** | Up to 1 year | Exactly-once | Full (stored for 90 days) | Per state transition | Long-running data pipelines |
| **Express (Async)** | Up to 5 minutes | At-least-once | CloudWatch only | Per execution + duration | High-volume event processing |
| **Express (Sync)** | Up to 5 minutes | At-least-once | CloudWatch only | Per execution + duration | Synchronous orchestration |

**For data pipelines**: Use Standard. ETL jobs run for minutes to hours — Express's 5-minute limit is too short.

---

## SDK Integrations

Step Functions can call AWS services directly — without a Lambda function as intermediary:

**Optimistic integration** (`.sync` suffix): Step Functions waits for the task to complete before moving to the next state.

```json
{
  "Resource": "arn:aws:states:::glue:startJobRun.sync",
  "Parameters": {"JobName": "my-etl-job"}
}
```

**Request-response**: Step Functions calls the API and immediately moves on — doesn't wait.

**Supported direct integrations:**
- AWS Glue (start job, start crawler)
- Amazon EMR (add step, create cluster)
- AWS Lambda (invoke)
- Amazon ECS (run task)
- AWS Batch (submit job)
- Amazon SQS (send message)
- Amazon SNS (publish)
- Amazon DynamoDB (put item, get item)
- Amazon S3 (put object, get object)
- Amazon Athena (start query execution)
- Amazon SageMaker (create training job, create transform job)

---

## Error Handling

### Retry

```json
"Retry": [
  {
    "ErrorEquals": ["Lambda.ServiceException", "Lambda.AWSLambdaException"],
    "IntervalSeconds": 2,
    "MaxAttempts": 3,
    "BackoffRate": 2
  }
]
```

Retries 3 times with exponential backoff (2s, 4s, 8s).

### Catch

```json
"Catch": [
  {
    "ErrorEquals": ["States.TaskFailed"],
    "ResultPath": "$.error",
    "Next": "HandleError"
  },
  {
    "ErrorEquals": ["States.ALL"],
    "Next": "FatalFailure"
  }
]
```

Routes to different states depending on the error type.

---

## Map State — Parallel Processing

Process each item in an array in parallel:

```json
{
  "ProcessFiles": {
    "Type": "Map",
    "ItemsPath": "$.files",
    "MaxConcurrency": 10,
    "Iterator": {
      "StartAt": "ProcessSingleFile",
      "States": {
        "ProcessSingleFile": {
          "Type": "Task",
          "Resource": "arn:aws:lambda:...:ProcessFile",
          "End": true
        }
      }
    }
  }
}
```

Input: `{"files": ["file1.csv", "file2.csv", "file3.csv"]}`  
Step Functions runs 3 parallel ProcessSingleFile tasks, up to 10 at once.

---

## Step Functions for Data Pipelines

### Typical ETL Pattern

```
EventBridge (daily schedule)
    ↓
Step Functions State Machine:
  1. StartCrawler (Task) → wait for completion
  2. Choice: crawler succeeded?
     ├── No → NotifyFailure → Fail
     └── Yes → RunETLJob (Task) → wait
  3. Choice: job succeeded?
     ├── No → RetryOrNotify
     └── Yes → RunDataQualityCheck (Task)
  4. Choice: data quality passed?
     ├── No → QuarantineData → NotifyTeam
     └── Yes → TriggerRedshiftLoad (Task)
  5. NotifySuccess → Succeed
```

### Step Functions vs MWAA vs Glue Workflows

| | Step Functions | MWAA (Airflow) | Glue Workflows |
|---|---|---|---|
| AWS-native | Yes | No (Airflow) | Yes |
| Multi-service | Yes | Yes | Glue services only |
| Visual editor | Yes | No | Limited |
| Open-source portability | No | Yes | No |
| Cost | Per transition | Always-on | Free (Glue cost only) |
| Complex DAG dependencies | Moderate | Excellent | Limited |

---

## Exam Scenarios

| Scenario | Answer |
|---|---|
| Orchestrate a Glue crawler → Glue job → Redshift load → SNS notification | AWS Step Functions |
| Process 1,000 S3 files in parallel, each through a Lambda function | Step Functions Map state |
| Pipeline fails on step 3; automatically retry 3 times with exponential backoff | Step Functions Retry configuration on the Task state |
| Multi-step pipeline that can run for up to 6 hours | Step Functions Standard workflow |
| Team uses Apache Airflow DAGs and wants to migrate to AWS without rewriting | Amazon MWAA (Managed Airflow) |
