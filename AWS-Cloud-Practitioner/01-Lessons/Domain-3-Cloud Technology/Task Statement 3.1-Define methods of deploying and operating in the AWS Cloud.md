# Task Statement 3.1: Define Methods of Deploying and Operating in the AWS Cloud

## What you'll learn
- The three ways to interact with AWS: Console, CLI, and SDKs
- Infrastructure as Code — why it matters and what tools AWS provides
- How Elastic Beanstalk simplifies application deployment
- Cloud, hybrid, and on-premises deployment models
- Connectivity options for getting your network into AWS

---

## How you interact with AWS

Every action in AWS — creating a server, uploading a file, changing a security rule — is ultimately an API call. AWS provides three ways to make those calls.

### AWS Management Console

The Console is the web-based graphical interface at aws.amazon.com. You log in, click around, and interact with services through a point-and-click UI.

Good for: exploring services, learning, one-time tasks, and viewing dashboards.

Not good for: repetitive work, large-scale operations, or anything that needs to be reproducible. If you click through 20 steps to configure a server, you'll have to remember and repeat all 20 steps when you need another one.

### AWS CLI (Command Line Interface)

The CLI is a tool you install on your computer that lets you control AWS services from a terminal using commands. `aws s3 cp myfile.txt s3://my-bucket/` copies a file to S3. `aws ec2 describe-instances` lists your running servers.

Good for: automation scripts, repetitive tasks, piping output into other tools, and anything where clicking a UI is too slow.

The CLI is how many developers and operations engineers interact with AWS day-to-day.

### SDKs (Software Development Kits)

SDKs let you call AWS APIs directly from your application code. AWS provides SDKs for Python (boto3), JavaScript, Java, .NET, Go, Ruby, and more.

Good for: building applications that need to interact with AWS services programmatically — uploading user files to S3, sending notifications via SNS, reading from DynamoDB, invoking Lambda functions.

If you're writing an application that stores user uploads in S3, you use the SDK in your application code to make that happen.

### AWS CloudShell

A browser-based terminal that gives you a pre-authenticated CLI environment in the AWS Console. No installation required. Useful for quick scripting tasks without needing to set up CLI credentials locally.

---

## Infrastructure as Code (IaC)

Clicking through the Console to build infrastructure creates a problem: you can't easily reproduce what you built, and small inconsistencies creep in over time. Infrastructure as Code solves this by defining your infrastructure in files — then running those files to build the environment automatically and consistently.

Benefits of IaC:
- **Reproducibility**: Run the same code and get an identical environment every time
- **Version control**: Infrastructure definitions live in git alongside application code
- **Faster disaster recovery**: If a region fails, run the code in another region
- **Consistency**: No "I forgot to change that setting" errors from manual steps

### AWS CloudFormation

CloudFormation is AWS's native IaC service. You write a template (in JSON or YAML) that describes the AWS resources you want — EC2 instances, VPCs, S3 buckets, security groups, and their configuration and relationships. CloudFormation reads the template and builds the infrastructure automatically.

A collection of resources built from a CloudFormation template is called a **stack**. To update infrastructure, you update the template and CloudFormation figures out what changed and makes only the necessary modifications.

CloudFormation handles dependencies automatically — if your EC2 instance needs to be in a VPC, and that VPC needs to exist first, CloudFormation creates the VPC before the instance.

### AWS CDK (Cloud Development Kit)

The CDK lets you define CloudFormation infrastructure using familiar programming languages (Python, TypeScript, Java, etc.) instead of JSON or YAML. The CDK converts your code into a CloudFormation template and deploys it. Better for developers who prefer working in code rather than configuration file formats.

---

## Elastic Beanstalk

Elastic Beanstalk is a platform-as-a-service that handles application deployment and infrastructure management for you. You provide your application code; Beanstalk handles provisioning the servers, load balancers, and auto-scaling — and keeps them running.

What Beanstalk manages for you:
- Provisioning EC2 instances
- Setting up load balancing and auto-scaling
- Monitoring application health
- Deploying new versions of your application

What you keep control of:
- The underlying EC2 instances (you can still access them if needed)
- Environment configuration
- Scaling rules

Beanstalk supports multiple platforms: Python, Node.js, Java, PHP, Ruby, .NET, Go, and Docker. You zip your code and upload it; Beanstalk handles the rest.

The key difference from Lambda (serverless): Beanstalk still runs on servers — it just manages them for you. Lambda runs without any servers at all.

---

## Deployment models

### All cloud (cloud-native)

Everything runs in AWS. No on-premises infrastructure. Used by most startups and newer applications. Maximum cloud benefits: pay-per-use, auto-scaling, global reach, no hardware to manage.

### Hybrid

Some resources in AWS, some on-premises. The most common model for established companies migrating to cloud. Sensitive data or legacy applications stay on-premises; new workloads or less-sensitive systems run in AWS.

Hybrid requires connectivity between on-premises and AWS — either via VPN or AWS Direct Connect.

### On-premises (private cloud)

Everything runs in the company's own data centers. Can still use virtualization and automation that resembles cloud practices (VMware, OpenStack), but none of the AWS services.

AWS Outposts brings AWS hardware into your data center, so you can run native AWS services on-premises — blurring the line between hybrid and on-premises for specific compliance or latency requirements.

---

## Connectivity options

### Internet

The default way to reach AWS services. Your CLI commands and SDK calls go over the public internet by default, encrypted via TLS/HTTPS. Works everywhere. Bandwidth and latency depend on your internet connection.

### AWS VPN

Encrypted tunnel over the internet between your on-premises network and your AWS VPC. Easy to set up, uses existing internet infrastructure.

**Site-to-Site VPN**: Connects your office network to AWS. Traffic looks like it never left your corporate network.

**Client VPN**: Connects individual devices (laptops) to AWS. Useful for remote workers who need to access resources in a VPC.

VPN is still going over the internet — the traffic is encrypted, but it shares bandwidth with everything else on your internet connection.

### AWS Direct Connect

A dedicated private physical connection between your data center and AWS, bypassing the public internet entirely. Ordered through an AWS Direct Connect partner who physically provisions a fiber connection.

More expensive and slower to provision than VPN (weeks or months for physical setup), but offers consistent, predictable network performance — important for high-volume data transfer or latency-sensitive workloads.

---

## Exam focus

| Concept | What to know |
|---------|-------------|
| Console vs CLI vs SDK | Console = web GUI; CLI = terminal commands; SDK = in-code API calls |
| CloudFormation | IaC using JSON/YAML templates; reproducible infrastructure |
| Elastic Beanstalk | Upload code, AWS manages the platform; good for traditional web apps |
| Cloud vs hybrid vs on-premises | Cloud = all in AWS; hybrid = some in AWS, some on-premises; on-premises = all local |
| VPN vs Direct Connect | VPN = encrypted internet tunnel (quick); Direct Connect = dedicated private line (consistent) |

---

## Practice questions

**Q1.** A developer wants to write a Python script that automatically uploads files from a local server to S3 every night. Which AWS interaction method should they use?

A) AWS Management Console  
B) AWS SDK for Python (boto3)  
C) AWS CloudFormation  
D) Elastic Beanstalk

**Answer: B** — SDKs allow application code to call AWS APIs directly. boto3 is the Python SDK. The Console requires a human clicking; CloudFormation provisions infrastructure; Beanstalk manages application deployments, not scripted file transfers.

---

**Q2.** A company has 50 developers, each needing identical development environments with the same EC2 instances, VPCs, and security groups. New environments take 3 hours each to set up manually and often have configuration errors. What is the best solution?

A) Train developers to use the Console more carefully  
B) Use AWS CloudFormation to create a template that provisions the environment automatically  
C) Use AWS Elastic Beanstalk for all development work  
D) Hire more operations staff to set up environments

**Answer: B** — CloudFormation templates define infrastructure as code. Run the same template for each developer and get an identical, consistent environment in minutes. Eliminates manual setup time and configuration errors.

---

**Q3.** A financial company needs to connect their on-premises data center to AWS. They require a consistent, dedicated network connection with predictable throughput for large daily data transfers. Which connectivity option is most appropriate?

A) Public internet  
B) AWS Site-to-Site VPN  
C) AWS Direct Connect  
D) AWS Client VPN

**Answer: C** — Direct Connect provides a dedicated private connection with consistent, predictable bandwidth. VPN goes over the internet, which has variable performance. For large, regular data transfers where consistent throughput matters, Direct Connect is the right choice.

---

**Next:** [Task 3.2 — AWS Global Infrastructure](./Task%20Statement%203.2-Define%20the%20AWS%20global%20infrastructure.md)
