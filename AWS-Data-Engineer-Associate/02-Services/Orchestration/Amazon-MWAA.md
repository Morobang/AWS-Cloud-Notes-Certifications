# Amazon MWAA (Managed Workflows for Apache Airflow)

**Category**: Managed Workflow Orchestration  
**Exam weight**: Medium — know when MWAA fits vs Step Functions, and Airflow DAG basics

---

## What Is It?

Amazon MWAA is a managed Apache Airflow service. Apache Airflow is the industry-standard open-source workflow orchestration platform used by data engineering teams worldwide. MWAA lets you run Airflow on AWS without managing the server infrastructure.

---

## Apache Airflow Core Concepts

### DAG (Directed Acyclic Graph)

A DAG is a Python file that defines a workflow:
- **Directed**: Tasks have a defined execution order
- **Acyclic**: No circular dependencies (A → B → A is not allowed)
- **Graph**: Nodes are tasks, edges are dependencies

```python
from airflow import DAG
from airflow.operators.python import PythonOperator
from airflow.providers.amazon.aws.operators.glue import GlueJobOperator
from airflow.providers.amazon.aws.sensors.s3 import S3KeySensor
from datetime import datetime, timedelta

with DAG(
    dag_id='daily_etl_pipeline',
    schedule_interval='0 6 * * *',  # 6 AM UTC daily
    start_date=datetime(2024, 1, 1),
    catchup=False,
    default_args={
        'retries': 2,
        'retry_delay': timedelta(minutes=5)
    }
) as dag:

    # Wait for raw data to arrive in S3
    wait_for_data = S3KeySensor(
        task_id='wait_for_raw_data',
        bucket_name='my-data-lake',
        bucket_key='bronze/events/{{ ds }}/*.json',
        timeout=3600  # Wait up to 1 hour
    )

    # Run Glue ETL job
    run_glue_job = GlueJobOperator(
        task_id='transform_events',
        job_name='transform-daily-events',
        script_args={'--date': '{{ ds }}'}
    )

    # Dependencies: wait_for_data must complete before run_glue_job
    wait_for_data >> run_glue_job
```

### Operators

An operator is a template for a specific type of task:

| Operator | What it does |
|---|---|
| `PythonOperator` | Run a Python function |
| `BashOperator` | Run a bash command |
| `GlueJobOperator` | Start an AWS Glue ETL job |
| `AthenaOperator` | Run an Athena SQL query |
| `EmrAddStepsOperator` | Add a step to an EMR cluster |
| `S3CopyObjectOperator` | Copy an S3 object |
| `LambdaInvokeFunctionOperator` | Invoke a Lambda function |
| `StepFunctionStartExecutionOperator` | Start a Step Functions execution |

### Sensors

Sensors wait for a condition to be true:

| Sensor | What it waits for |
|---|---|
| `S3KeySensor` | An S3 object to exist |
| `GlueJobSensor` | A Glue job to complete |
| `ExternalTaskSensor` | Another Airflow DAG task to complete |
| `HttpSensor` | An HTTP endpoint to return a specific response |

### Templating (Jinja)

Airflow renders task parameters using Jinja templates at execution time:

| Template variable | Value |
|---|---|
| `{{ ds }}` | Execution date (`2024-01-15`) |
| `{{ ds_nodash }}` | Execution date no dashes (`20240115`) |
| `{{ ts }}` | Execution timestamp |
| `{{ prev_ds }}` | Previous execution date |
| `{{ next_ds }}` | Next execution date |

---

## MWAA Architecture

MWAA runs Airflow components on your behalf:
- **Scheduler**: Parses DAGs, schedules task execution
- **Workers**: Execute tasks (on managed Fargate containers)
- **Web server**: Airflow UI accessible via your VPC

**DAG storage**: DAGs are Python files stored in S3. MWAA watches an S3 bucket for changes and loads new DAGs automatically.

**Dependencies**: Install Python packages via `requirements.txt` in S3.

**Environment sizes**: Small (Development), Medium, Large, X-Large — determines how many concurrent DAGs and tasks can run.

---

## MWAA vs Step Functions

| | Amazon MWAA | AWS Step Functions |
|---|---|---|
| Underlying tech | Apache Airflow (open-source) | AWS proprietary |
| Portability | Yes — Airflow runs anywhere | No — AWS-specific |
| Learning curve | Higher (Airflow concepts, Python) | Lower (visual UI) |
| Ecosystem | Huge (thousands of operators/hooks) | AWS service integrations |
| Cost | Always-on environment (even when idle) | Per state transition |
| Complex dependencies | Excellent (Airflow's strength) | Moderate |
| Event-driven | Possible (sensors, triggers) | Native (EventBridge) |
| When to choose | Existing Airflow team/DAGs, complex DAG dependencies | New AWS project, simpler workflows |

---

## MWAA in Practice

### Backfill

Run historical DAG executions for a date range:

```bash
airflow dags backfill daily_etl_pipeline --start-date 2024-01-01 --end-date 2024-01-31
```

This runs the DAG for every day in January, using the `{{ ds }}` variable to process the correct date partition.

### XCom (Cross-Task Communication)

Pass small values between tasks:

```python
def get_record_count(**context):
    count = 1500000
    context['task_instance'].xcom_push(key='record_count', value=count)

def log_count(**context):
    count = context['task_instance'].xcom_pull(task_ids='count_records', key='record_count')
    print(f"Processed {count} records")
```

**Only for small values** (not large DataFrames). For large data, write to S3 and pass the S3 path via XCom.

---

## Exam Scenarios

| Scenario | Answer |
|---|---|
| Team has 50 existing Airflow DAGs. They want to run them on AWS with minimal changes. | Amazon MWAA — run existing DAGs unchanged |
| Wait for a specific S3 file to exist before starting an ETL job | MWAA S3KeySensor (or Step Functions S3 event trigger) |
| Reprocess 6 months of historical data through a daily data pipeline | MWAA backfill command |
| Simple 3-step pipeline: Glue crawler → Glue job → SNS notification | Step Functions (simpler, cheaper than MWAA for short pipelines) |
| Pipeline needs to wait for another team's pipeline to finish before starting | MWAA ExternalTaskSensor |
