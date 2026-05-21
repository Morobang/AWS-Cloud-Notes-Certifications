# Task 1.3: Orchestrate Data Pipelines

## Learning Objectives
- Design pipeline workflows using AWS Step Functions
- Use Amazon MWAA (Managed Apache Airflow) for DAG-based orchestration
- Trigger pipelines based on events vs schedules
- Handle pipeline failures and retries

---

## Why Orchestration Matters

A data pipeline is rarely a single step. Typically you need to:
1. Run a crawler to discover new data
2. Run a Glue job to transform it
3. Load it to Redshift
4. Run data quality checks
5. Notify the team when done (or alert if something fails)

Orchestration ties these steps together, manages dependencies, handles errors, and retries failures automatically.

---

## AWS Step Functions

### What It Is

A serverless visual workflow service. You define states (tasks, choices, waits, parallels) in JSON (Amazon States Language) and Step Functions executes them, managing the state transitions.

### Core Concepts

**States:**
| State type | Description |
|---|---|
| `Task` | Do work — call a Lambda, Glue job, ECS task, SNS, SQS, etc. |
| `Choice` | Branch based on a condition (if/else) |
| `Wait` | Pause for a specific time or until a timestamp |
| `Parallel` | Run multiple branches simultaneously |
| `Map` | Process each item in a list |
| `Pass` | Pass input to output unchanged (useful for testing) |
| `Succeed` | Terminal success state |
| `Fail` | Terminal failure state |

**State Machine**: The complete workflow definition (all states + transitions).

**Execution**: One run of the state machine with a specific input.

### Workflow Types

| Type | Use case |
|---|---|
| **Standard** | Long-running (up to 1 year), exactly-once execution, full audit history |
| **Express** | High-volume, short-duration (up to 5 min), at-least-once, lower cost |

### Error Handling

Step Functions has built-in retry and catch:

```json
{
  "Retry": [
    {
      "ErrorEquals": ["States.TaskFailed"],
      "IntervalSeconds": 5,
      "MaxAttempts": 3,
      "BackoffRate": 2
    }
  ],
  "Catch": [
    {
      "ErrorEquals": ["States.ALL"],
      "Next": "NotifyFailure"
    }
  ]
}
```

This retries 3 times with exponential backoff, then routes to a failure notification state.

### Typical Data Pipeline Pattern

```
EventBridge (scheduled trigger)
       ↓
Step Functions State Machine:
  1. StartGlueCrawler → wait for completion
  2. Check crawler status (Choice state)
     ├── Success → StartGlueJob
     └── Failed → NotifySlack
  3. StartGlueJob → wait for completion
  4. Check job status (Choice state)
     ├── Success → TriggerRedshiftLoad → NotifySuccess
     └── Failed → NotifyFailure
```

---

## Amazon MWAA — Managed Apache Airflow

### What It Is

Amazon MWAA is managed Apache Airflow — the industry-standard open-source workflow orchestration tool. If your team already uses or knows Airflow, MWAA lets you run it on AWS without managing the servers.

### When to Use MWAA vs Step Functions

| Factor | Step Functions | MWAA |
|---|---|---|
| Existing Airflow knowledge | No | Yes |
| Open-source portability | No — AWS specific | Yes — standard Airflow DAGs |
| Complexity | Lower (visual UI) | Higher (Python DAGs) |
| Community/ecosystem | AWS SDK integrations | Huge open-source ecosystem |
| Cost | Pay per state transition | Always-on managed environment ($$$) |
| Complex dependency management | Moderate | Excellent (Airflow's strength) |

**Choose MWAA when**: You have Airflow DAGs today, your team knows Python/Airflow, you need complex dependency management, or you want open-source portability.  
**Choose Step Functions when**: AWS-native project, simpler workflows, you want visual workflow builder, lower cost.

### Airflow Key Concepts

**DAG** (Directed Acyclic Graph): The workflow definition — a Python file that defines tasks and their dependencies.

**Operator**: A template for a task (e.g., `GlueJobOperator`, `AthenaOperator`, `PythonOperator`).

**Sensor**: Waits for a condition (e.g., `S3KeySensor` waits until a file appears in S3).

**Example DAG structure:**
```python
from airflow import DAG
from airflow.providers.amazon.aws.operators.glue import GlueJobOperator
from airflow.providers.amazon.aws.sensors.s3 import S3KeySensor

with DAG("daily_etl", schedule_interval="0 2 * * *") as dag:  # 2am daily
    
    wait_for_file = S3KeySensor(
        task_id="wait_for_raw_data",
        bucket_name="my-data-lake",
        bucket_key="raw/events/{{ ds }}/*.parquet"
    )
    
    transform_data = GlueJobOperator(
        task_id="transform_events",
        job_name="events-transform-job",
        script_args={"--date": "{{ ds }}"}
    )
    
    # Dependency: wait_for_file must succeed before transform_data runs
    wait_for_file >> transform_data
```

---

## AWS Glue Workflows

For simpler, Glue-only pipelines, AWS Glue has its own built-in orchestration: **Glue Workflows**.

You can chain:
- **Crawlers** → run on a schedule or trigger
- **Glue Jobs** → transform data
- **Triggers** → conditions that start the next step

Good for: Glue-only pipelines without needing Step Functions or Airflow.  
Limitation: Only Glue components. For multi-service orchestration (Glue + EMR + Lambda + SNS), use Step Functions.

---

## EventBridge — Trigger-Based Orchestration

Many pipelines are triggered by events rather than fixed schedules:

| Trigger | Service | Example |
|---|---|---|
| Fixed schedule | EventBridge Scheduler | "Run ETL every day at 6 AM" |
| New S3 file arrives | S3 Event Notification → EventBridge | "New customer data uploaded" |
| Database change | DynamoDB Streams → Lambda | "New order placed" |
| Pipeline completion | Step Functions → EventBridge → next pipeline | "ETL done, trigger ML training" |

**EventBridge rules** match events and route them to targets (Step Functions, Lambda, Glue, SQS, etc.).

---

## Pipeline Design Best Practices

### Idempotency
Design steps so they can be safely re-run without duplicating data. If a job fails halfway and restarts, it should produce the same result as a successful first run.

Approaches:
- Overwrite (not append) to S3 partitions
- Use `UPSERT` in Redshift instead of `INSERT`
- Track processed records in a status table

### Observability
Always include:
- CloudWatch metrics on job duration and record counts
- Alerts when jobs fail or exceed expected runtime
- Dead-letter queues (DLQ) for failed records in Kinesis/SQS

### Backfilling
Design pipelines to accept a date parameter so you can reprocess historical data when you fix a bug or change transformation logic.

```python
# Glue job that accepts a date parameter
import sys
from awsglue.utils import getResolvedOptions

args = getResolvedOptions(sys.argv, ['date'])
processing_date = args['date']  # e.g., "2024-01-15"

# Read only that day's partition
input_path = f"s3://my-lake/bronze/events/date={processing_date}/"
```
