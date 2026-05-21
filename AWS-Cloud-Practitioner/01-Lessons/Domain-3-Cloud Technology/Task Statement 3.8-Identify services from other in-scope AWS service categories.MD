# Task Statement 3.8: Identify Services from Other In-Scope AWS Service Categories

## What you'll learn
- Application integration: SNS, SQS, EventBridge, Step Functions
- Developer tools and CI/CD: CodeCommit, CodeBuild, CodeDeploy, CodePipeline
- Migration services: DMS, Application Migration Service
- End-user computing: WorkSpaces, AppStream
- Management and governance: Systems Manager, Control Tower, AWS Health Dashboard
- Business applications: Amazon Connect

---

## Application integration

When applications need to communicate with each other, directly calling from one to another creates tight coupling — if one service fails, the caller fails too. AWS messaging services decouple components so they communicate asynchronously.

### Amazon SNS — Simple Notification Service

SNS is a publish/subscribe messaging service. A publisher sends a message to a topic; all subscribers to that topic receive it simultaneously.

**What it enables**: Fan-out — one event triggers multiple actions. An order placed triggers: send confirmation email, update inventory, notify the warehouse, log to analytics — all at once.

**Subscribers can be**: Email addresses, SMS numbers, Lambda functions, SQS queues, HTTP endpoints.

*Use when*: You need to notify multiple consumers about the same event simultaneously.

### Amazon SQS — Simple Queue Service

SQS is a managed message queue. Producers send messages to a queue; consumers poll the queue and process messages at their own pace.

**Key feature**: Decoupling. If a downstream service is slow or temporarily down, messages accumulate in the queue and get processed when the service recovers. No messages are lost.

**Standard vs FIFO queues**:
- **Standard**: Best-effort ordering, at-least-once delivery. Highest throughput.
- **FIFO**: Exactly-once processing, strict ordering. Lower throughput. Used when order matters.

*Use when*: Smoothing out traffic spikes, ensuring requests aren't lost if a service is temporarily unavailable, decoupling producer and consumer processing speeds.

### Amazon EventBridge

EventBridge is a serverless event bus. Applications publish events; EventBridge routes them to targets based on rules.

**What makes it different from SNS**: EventBridge understands event content and can filter/route based on the data inside events. SNS fans out to all subscribers regardless of content.

**Common sources**: AWS services (EC2 state changes, S3 uploads, RDS events), SaaS applications (Salesforce, Zendesk), and your own applications.

*Use when*: Building event-driven architectures where different events should trigger different actions based on event content.

### AWS Step Functions

Step Functions coordinates multiple AWS services into visual workflows (state machines). Each step calls a Lambda function, invokes another service, or makes a decision.

*Use when*: Orchestrating multi-step processes — order fulfillment (validate → charge → ship → notify), document processing pipelines, approval workflows. Instead of writing complex coordination logic in application code, you define it as a visual workflow.

---

## Developer tools and CI/CD

CI/CD (Continuous Integration / Continuous Deployment) automates the process of building, testing, and deploying code. AWS provides a suite of tools that cover each stage.

### AWS CodeCommit

Managed private Git repository in AWS. Source code hosting — like GitHub or GitLab, but running in your AWS account.

*Note*: AWS announced plans to wind down new customer sign-ups for CodeCommit. Many teams use GitHub, GitLab, or Bitbucket instead, but CodeCommit still appears on the exam.

### AWS CodeBuild

Managed build service. Compiles source code, runs tests, and produces deployment packages. Scales automatically — no build servers to manage.

### AWS CodeDeploy

Managed deployment service. Automates code deployments to EC2 instances, Lambda functions, and ECS. Handles rolling updates, blue/green deployments, and automatic rollback on failure.

### AWS CodePipeline

The orchestrator that connects all the stages. A pipeline defines the full workflow: code committed to CodeCommit → CodeBuild compiles and tests → CodeDeploy deploys to production. Every code commit automatically flows through the pipeline.

**The four services together**: CodeCommit (store) → CodeBuild (build/test) → CodeDeploy (deploy) → CodePipeline (orchestrate the whole process).

---

## Migration services

### AWS Database Migration Service (DMS)

DMS migrates databases to AWS with minimal downtime. It can move data continuously while your source database remains operational, so migration doesn't require a maintenance window.

**Supported migrations**: Oracle to RDS for Oracle, MySQL to Aurora, SQL Server to RDS for SQL Server, and many cross-engine migrations (Oracle to PostgreSQL, for example — though schema conversion requires the Schema Conversion Tool).

*Use when*: Migrating databases from on-premises to RDS or Aurora, migrating between database engines.

### AWS Application Migration Service (MGN)

Formerly CloudEndure Migration. Automates the lift-and-shift migration of on-premises servers to EC2 instances. It continuously replicates your source servers in the background; when you're ready to cut over, it launches converted EC2 instances with minimal downtime.

*Use when*: Migrating physical or virtual servers to EC2 without redesigning applications.

---

## End-user computing

### Amazon WorkSpaces

WorkSpaces provides managed virtual desktops — Windows or Linux desktops running in the cloud, accessible from any device. Users connect to their WorkSpace from a laptop, tablet, or thin client.

*Use when*: Remote workforce needing secure access to corporate applications and data, call center agents, developers needing standardized environments, compliance scenarios requiring data to stay in AWS rather than on user devices.

### Amazon AppStream 2.0

AppStream streams desktop applications to users' browsers — without delivering a full virtual desktop. Individual applications (AutoCAD, data analysis tools) run in AWS and are streamed to any browser.

*Use when*: Delivering specific applications to users without managing virtual desktops. Good for partners or contractors who need access to one application, not a full Windows desktop.

---

## Management and governance

### AWS Systems Manager (SSM)

Systems Manager provides operational tools for managing AWS and on-premises infrastructure at scale:

- **Session Manager**: Securely connect to EC2 instances via browser or CLI without opening SSH/RDP ports or managing SSH keys
- **Patch Manager**: Automate OS and application patching across fleets
- **Parameter Store**: Store configuration data and secrets (similar to Secrets Manager but simpler, for non-secret config values)
- **Run Command**: Execute scripts across multiple instances simultaneously

*Use when*: Managing large fleets of EC2 instances, automating patching, accessing instances without exposing SSH.

### AWS Control Tower

Control Tower automates the setup of a well-architected multi-account AWS environment. It creates a "landing zone" — a pre-configured set of accounts (Management, Log Archive, Audit) with guardrails enforced via AWS Organizations and SCPs.

*Use when*: Starting a new multi-account AWS environment and wanting to follow best practices from day one rather than building governance incrementally.

### AWS Health Dashboard

The Health Dashboard has two views:

**Service Health Dashboard**: Shows real-time status of all AWS services globally. When AWS has an outage or service disruption, this is where it's publicly reported.

**Personal Health Dashboard** (AWS Health): Shows events and issues that specifically affect your AWS account and resources — notifications about maintenance, service degradations impacting your instances, and required actions.

---

## Business applications

### Amazon Connect

Connect is a cloud-based contact center service. Companies set up and manage call centers using Connect — defining call flows, managing agents, recording calls, and analyzing customer interactions.

Integrates with Amazon Lex (for IVR/chatbot flows) and Amazon Transcribe/Comprehend (for call analysis and sentiment tracking).

*Use when*: Building or migrating a customer service call center to the cloud.

---

## Service category quick reference

| Category | Service | What it does |
|----------|---------|-------------|
| Messaging | SNS | Pub/sub notifications to multiple subscribers |
| Messaging | SQS | Message queue for async decoupling |
| Events | EventBridge | Event bus with content-based routing |
| Orchestration | Step Functions | Visual workflows for multi-step processes |
| Source control | CodeCommit | Managed Git repository |
| Build | CodeBuild | Managed code compilation and testing |
| Deploy | CodeDeploy | Automated deployments to EC2/Lambda/ECS |
| Pipeline | CodePipeline | CI/CD orchestration across all stages |
| DB migration | DMS | Move databases to AWS with minimal downtime |
| Server migration | MGN | Lift-and-shift servers to EC2 |
| Virtual desktops | WorkSpaces | Full cloud desktops for remote workers |
| App streaming | AppStream 2.0 | Stream individual apps to browsers |
| Fleet management | Systems Manager | Patch, manage, and access EC2 fleets |
| Multi-account setup | Control Tower | Automated landing zone with guardrails |
| AWS status | Health Dashboard | Service health and personal account alerts |
| Contact center | Amazon Connect | Cloud-based call center |

---

## Practice questions

**Q1.** An e-commerce company processes orders. When an order is placed, they need to simultaneously: send a confirmation email, update inventory, and notify the warehouse. All three systems should receive the same order event. Which service enables this?

A) Amazon SQS  
B) Amazon SNS  
C) AWS Step Functions  
D) Amazon EventBridge

**Answer: B** — SNS fan-out delivers the same message to multiple subscribers simultaneously. The three downstream systems (email, inventory, warehouse) each subscribe to the order topic and receive the same notification at the same time. SQS delivers each message to one consumer. Step Functions orchestrates sequential workflows. EventBridge routes events based on rules — suitable if they need filtering, but SNS is simpler for pure fan-out.

---

**Q2.** A developer wants to implement a CI/CD pipeline where: code is committed to a repository → automatically builds and runs tests → deploys to production if tests pass. Which AWS services form this pipeline?

A) CloudFormation + Elastic Beanstalk  
B) CodeCommit + CodeBuild + CodeDeploy + CodePipeline  
C) Lambda + S3 + CloudFront  
D) EC2 + Auto Scaling + CloudWatch

**Answer: B** — CodePipeline orchestrates the full workflow. CodeCommit stores the code. CodeBuild compiles and tests. CodeDeploy deploys to the target environment. These four services are the AWS native CI/CD toolchain.

---

**Q3.** An organization wants to provide remote workers with full Windows desktop environments that they can access from home using personal laptops. Corporate data should remain in AWS, not on employees' personal devices. Which service fits?

A) Amazon AppStream 2.0  
B) Amazon WorkSpaces  
C) AWS Elastic Beanstalk  
D) Amazon EC2 with Remote Desktop

**Answer: B** — WorkSpaces provides complete virtual desktop environments — the full Windows OS — accessible from any device. Data stays in AWS. AppStream 2.0 streams individual applications, not full desktops. Beanstalk manages web application deployments. EC2 with RDP could technically work but is unmanaged and significantly more complex than WorkSpaces.

---

**Next:** [Domain 4 — Billing, Pricing, and Support](../../Domain-4-Billing%20Pricing%20and%20Support/README.md)
