# Task 3.1: Automate Data Processing

## Learning Objectives
- Apply CI/CD practices to data pipelines using CodePipeline, CodeBuild, and CodeDeploy
- Use CloudFormation and CDK to deploy pipeline infrastructure as code
- Automate Glue, EMR, Lambda, and Batch jobs through schedulers and event triggers
- Understand AWS Batch for high-volume batch workloads

---

## Why Automation Matters for Data Pipelines

Manual data pipeline runs create risk:
- Engineers forget to run nightly jobs
- A schema change breaks a downstream pipeline with no notification
- Deploying a new version of a Glue script requires SSH'ing into a console

Automated pipelines:
- Run on schedule or event trigger without human intervention
- Deploy new code through a tested, reviewed process
- Alert on failure, retry automatically

---

## CI/CD for Data Pipelines

### The Data Pipeline Deployment Problem

A "data pipeline" typically involves:
- Glue ETL scripts (Python/Scala)
- CloudFormation templates (infrastructure)
- Lambda function code
- Step Functions state machine definitions

All of these need version control, testing, and a deployment process.

### AWS CodePipeline

CodePipeline orchestrates the CI/CD process. For a data pipeline:

```
Source (CodeCommit/GitHub)
    ↓
Build (CodeBuild)
  - Run unit tests on Glue scripts
  - Package Lambda functions
  - Validate CloudFormation templates (cfn-lint)
    ↓
Test (CodeBuild)
  - Deploy to dev environment
  - Run integration tests against test data
    ↓
Deploy (CloudFormation / CodeDeploy)
  - Deploy updated Glue scripts to S3
  - Update Lambda function code
  - Deploy Step Functions state machine
```

### AWS CodeBuild — Testing Glue Scripts

CodeBuild runs build and test phases in a managed container.

**buildspec.yml for a Glue ETL project:**
```yaml
version: 0.2

phases:
  install:
    runtime-versions:
      python: 3.9
    commands:
      - pip install pytest pyspark awsglue-local

  build:
    commands:
      - echo "Running unit tests..."
      - pytest tests/ -v

  post_build:
    commands:
      - echo "Uploading Glue scripts to S3..."
      - aws s3 sync glue_scripts/ s3://my-pipeline-bucket/scripts/

artifacts:
  files:
    - "**/*"
```

**Testing Glue scripts locally**: Use the `awsglue-local` package to run Glue transforms against sample data without a live Glue endpoint.

### Infrastructure as Code for Data Pipelines

#### AWS CloudFormation

Define all pipeline infrastructure — S3 buckets, Glue jobs, IAM roles, Lambda functions — in a YAML/JSON template.

```yaml
Resources:
  GlueETLJob:
    Type: AWS::Glue::Job
    Properties:
      Name: daily-events-transform
      Role: !GetAtt GlueServiceRole.Arn
      Command:
        Name: glueetl
        ScriptLocation: !Sub "s3://${ScriptsBucket}/scripts/transform_events.py"
        PythonVersion: "3"
      DefaultArguments:
        "--job-language": "python"
        "--enable-metrics": ""
      MaxRetries: 2
      NumberOfWorkers: 10
      WorkerType: G.1X
```

Benefits: Every environment (dev, staging, prod) is created from the same template — no configuration drift.

#### AWS CDK (Cloud Development Kit)

CDK lets you define infrastructure in Python, TypeScript, Java, or C#. For data engineers comfortable with Python, CDK is often preferable to raw CloudFormation YAML.

```python
from aws_cdk import aws_glue as glue, aws_s3 as s3

# Create a Glue job in CDK
job = glue.CfnJob(
    self, "DailyETL",
    name="daily-events-transform",
    role=glue_role.role_arn,
    command=glue.CfnJob.JobCommandProperty(
        name="glueetl",
        script_location=f"s3://{scripts_bucket.bucket_name}/transform.py",
        python_version="3"
    ),
    number_of_workers=10,
    worker_type="G.1X"
)
```

CDK synthesizes to CloudFormation — you get the benefits of a real programming language (loops, conditions, reusable constructs) while deploying via CloudFormation.

---

## Automating Pipeline Execution

### EventBridge Scheduler

Schedule any AWS service call on a cron or rate basis:

```
rate(1 day)       → trigger Step Functions execution daily
cron(0 6 * * ? *) → trigger Lambda at 6 AM UTC every day
rate(1 hour)      → run Glue Crawler hourly
```

**EventBridge rules** react to events (not just schedules):

| Event source | Example trigger |
|---|---|
| S3 `ObjectCreated` | New file landed → start Glue job |
| Glue job `SUCCEEDED` | ETL done → trigger Redshift COPY |
| DynamoDB Streams | Record inserted → Lambda processes it |
| CodePipeline stage change | Deployment complete → notify Slack |

### AWS Lambda for Event-Driven Automation

Lambda is often the glue between event triggers and pipeline actions:

```python
import boto3

def handler(event, context):
    glue = boto3.client('glue')
    
    # Extract S3 key from S3 event notification
    bucket = event['Records'][0]['s3']['bucket']['name']
    key = event['Records'][0]['s3']['object']['key']
    
    # Start Glue job with file path as argument
    glue.start_job_run(
        JobName='process-incoming-file',
        Arguments={
            '--input_path': f's3://{bucket}/{key}',
            '--output_path': 's3://processed-data/silver/'
        }
    )
```

---

## AWS Batch — Managed Batch Computing

### What Is AWS Batch?

AWS Batch runs batch computing jobs on managed EC2 or Fargate compute. You submit jobs (Docker containers), and Batch provisions the right amount of compute, runs the jobs, and terminates when done.

**Key components:**

| Component | Description |
|---|---|
| **Job definition** | Docker image + CPU/memory requirements + command |
| **Job queue** | Where jobs wait before running; linked to compute environment |
| **Compute environment** | Managed EC2 (Spot or On-Demand) or Fargate |
| **Array job** | Submit N copies of a job with different parameters (e.g., process each month in parallel) |

### Batch vs Glue vs Lambda

| | AWS Batch | AWS Glue | Lambda |
|---|---|---|---|
| Runtime | Any Docker container | Spark (Python/Scala) | Python/Node/Java (managed) |
| Duration | Hours to days | Minutes to hours | Max 15 minutes |
| Use case | Custom code, long-running batch | Spark ETL, Data Catalog integration | Short event-driven transforms |
| Custom libraries | Yes — build any Docker image | Limited | Yes (Lambda layers) |
| Cost | EC2 Spot pricing | DPU-hours | Per invocation + duration |

**Choose Batch when**: Your transformation code runs in a Docker container, needs full control over the runtime environment, or runs for hours.

**Common Batch patterns:**
- End-of-month financial processing (runs once a month, takes 6 hours)
- ML model training on GPU instances
- Video transcoding
- Complex scientific simulations

### Array Jobs Pattern

Run 365 independent jobs (one per day of the year) in parallel:

```python
import boto3
batch = boto3.client('batch')

batch.submit_job(
    jobName='annual-reprocessing',
    jobQueue='data-engineering-queue',
    jobDefinition='reprocess-events',
    arrayProperties={'size': 365},  # Creates 365 child jobs
    parameters={'year': '2024'}
    # Each child job receives AWS_BATCH_JOB_ARRAY_INDEX env var (0-364)
    # Job uses index to compute which day to process
)
```

---

## AWS SAM (Serverless Application Model)

SAM simplifies deploying serverless data processing applications (Lambda + API Gateway + DynamoDB):

```yaml
# template.yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31

Resources:
  DataProcessorFunction:
    Type: AWS::Serverless::Function
    Properties:
      Runtime: python3.11
      Handler: app.handler
      Events:
        S3Trigger:
          Type: S3
          Properties:
            Bucket: !Ref DataBucket
            Events: s3:ObjectCreated:*
            Filter:
              S3Key:
                Rules:
                  - Name: prefix
                    Value: "raw/"
```

SAM extends CloudFormation — a single template defines Lambda code + triggers + IAM roles. `sam deploy` packages and deploys everything.

---

## Exam Scenarios

**"A company wants to automatically start a Glue ETL job whenever a new CSV file is uploaded to S3."**
→ S3 Event Notification → EventBridge rule → Glue job (or S3 Event Notification → Lambda → `glue.start_job_run()`)

**"A team writes Glue ETL scripts in Python. They want code review, automated testing, and a repeatable deployment process."**
→ CodeCommit → CodePipeline → CodeBuild (run pytest) → deploy scripts to S3

**"A data processing job runs for 3 hours using custom Python code in a Docker container. It runs once at end of month."**
→ AWS Batch (long-running custom container)

**"A team wants to define their entire Glue job, Lambda function, and IAM roles in code that they can version control."**
→ CloudFormation or AWS CDK
