# AWS CLI (Command Line Interface)

## What you'll learn
- What the AWS CLI is and why it exists
- How the CLI relates to the AWS Management Console and SDKs
- Common CLI use cases
- Key exam facts

---

## What is the AWS CLI?

The AWS Command Line Interface (CLI) is an open-source tool that lets you interact with AWS services directly from your terminal or command prompt. Instead of clicking through the AWS Management Console, you type commands that call the same underlying AWS APIs.

The CLI is available for Windows, macOS, and Linux.

---

## Three ways to interact with AWS

| Method | How it works | Best for |
|--------|-------------|---------|
| AWS Management Console | Web browser GUI | Exploration, one-off tasks, learning |
| AWS CLI | Commands in a terminal | Scripting, automation, bulk operations |
| AWS SDKs | Code in Python, Java, Node.js, etc. | Application-level API calls from code |

All three call the same AWS APIs — the CLI is simply a thin wrapper around those APIs.

---

## How the CLI works

1. You install the CLI on your machine
2. You configure it with credentials (access key ID + secret access key) using `aws configure`
3. You run commands in the format: `aws <service> <action> [options]`

Examples:
- `aws s3 ls` — list all S3 buckets
- `aws ec2 describe-instances` — list EC2 instances
- `aws s3 cp file.txt s3://my-bucket/` — copy a file to S3

---

## Common use cases

**Automation and scripting** — Write shell scripts that provision resources, upload files, start/stop instances, or trigger Lambda functions on a schedule.

**Infrastructure management** — Create, modify, and delete AWS resources without clicking through the console — faster and repeatable.

**CI/CD pipelines** — Deploy code to EC2, update Lambda functions, push Docker images to ECR from within build scripts.

**Bulk operations** — Apply the same change to many resources at once (e.g., add a tag to 50 EC2 instances).

---

## AWS CloudShell

AWS CloudShell is a browser-based shell environment available directly in the AWS Management Console. It comes with the AWS CLI pre-installed and pre-authenticated — no need to configure credentials. Useful for quick tasks without installing anything locally.

---

## Exam focus

- AWS CLI = **command-line tool** to control AWS services from a terminal
- Alternative to the console — same actions, different interface
- Uses **access keys** (access key ID + secret access key) for authentication
- **AWS CloudShell** = browser-based shell with CLI pre-installed in the console
- Key differentiator: Console is GUI; CLI is text/script-based; SDKs are code-integrated

---

## Practice questions

**Q1.** A developer wants to automate the process of copying files to Amazon S3 every night using a cron job on a Linux server. Which tool is most appropriate for this task?

A) AWS Management Console  
B) AWS CloudFormation  
C) AWS CLI  
D) AWS SDK for Python

**Answer: C** — The AWS CLI can be called from shell scripts and cron jobs to automate S3 operations like file uploads. The Management Console requires manual interaction. CloudFormation provisions infrastructure, not file transfers. The Python SDK would also work but requires writing a Python script rather than a simple shell command.
