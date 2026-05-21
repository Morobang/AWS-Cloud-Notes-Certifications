# Amazon CloudWatch

## What you'll learn
- What CloudWatch is and what it monitors
- Metrics, alarms, logs, and dashboards
- CloudWatch vs CloudTrail
- Key exam facts

---

## What is Amazon CloudWatch?

Amazon CloudWatch is a **monitoring and observability service** for AWS resources and applications. It collects metrics, logs, and events — then lets you visualize data, set alarms, and automate responses to operational changes.

CloudWatch is the central monitoring hub for your AWS environment.

---

## Key components

**Metrics** — Numeric measurements over time. Every AWS service publishes metrics to CloudWatch automatically:
- EC2: CPU utilization, network in/out, disk reads/writes
- RDS: database connections, read/write latency
- Lambda: invocations, duration, errors, throttles
- S3: bucket size, number of objects

You can also publish **custom metrics** from your application code.

**Alarms** — Trigger actions when a metric crosses a threshold:
- "If CPU > 80% for 5 minutes → send SNS notification"
- "If CPU > 70% for 2 minutes → trigger Auto Scaling scale-out action"
- Alarm states: OK, ALARM, INSUFFICIENT_DATA

**Logs** — CloudWatch Logs stores, monitors, and searches log files from EC2 instances, Lambda functions, CloudTrail, VPC Flow Logs, and any application that sends logs to the Log Agent.
- **Log groups**: Container for related log streams (e.g., all logs from one application)
- **Log streams**: Sequence of events from a single source (e.g., one EC2 instance)
- **Metric filters**: Extract metric data from log content (e.g., count ERROR lines per minute)

**Dashboards** — Visual panels showing metrics and alarms in a customizable view. Share operational dashboards with your team.

**CloudWatch Events / EventBridge** — Route events from AWS services (EC2 state change, CodePipeline state, etc.) to targets like Lambda or SNS.

---

## CloudWatch vs CloudTrail

| | Amazon CloudWatch | AWS CloudTrail |
|-|------------------|----------------|
| Purpose | Monitor operational metrics and logs | Record API calls (audit trail) |
| What it tracks | CPU, memory, errors, latency | Who called which API, when, from where |
| Use case | Performance monitoring, alarms | Security auditing, compliance |
| Question | "Is my application healthy?" | "Who changed this resource?" |

Both are essential but serve different purposes.

---

## When to use it

**Set alerts on resource health** — "Notify the on-call team when any Lambda function has errors for 5 consecutive minutes."

**Monitor custom application metrics** — Publish business metrics (orders per minute, active users) to CloudWatch for visibility.

**Log analysis** — Search Lambda logs, identify error patterns, create metric filters that alarm on specific log messages.

**Auto Scaling trigger** — CloudWatch alarms trigger Auto Scaling policies based on CPU or custom metrics.

---

## Exam focus

- CloudWatch = **monitoring** — metrics, alarms, logs, dashboards
- **Metrics**: numerical data points from AWS services and custom apps
- **Alarms**: trigger SNS notifications or Auto Scaling actions on threshold violations
- **Logs**: store and search log data from EC2, Lambda, CloudTrail, etc.
- Key differentiator from CloudTrail: CloudWatch monitors performance; CloudTrail records API calls for auditing

---

## Practice questions

**Q1.** A DevOps team wants to receive an email notification when the average CPU utilization of their EC2 instances exceeds 85% for 10 consecutive minutes. Which AWS service and feature combination enables this?

A) AWS CloudTrail with an event rule  
B) Amazon CloudWatch alarm with SNS notification  
C) AWS Config with a compliance rule  
D) AWS Trusted Advisor with a cost check

**Answer: B** — CloudWatch alarms monitor metrics (EC2 CPU utilization) and trigger actions (SNS notification, which can send email) when thresholds are breached. CloudTrail records API calls, not performance metrics. Config checks resource configuration compliance. Trusted Advisor checks best practices but doesn't configure custom threshold alarms.
