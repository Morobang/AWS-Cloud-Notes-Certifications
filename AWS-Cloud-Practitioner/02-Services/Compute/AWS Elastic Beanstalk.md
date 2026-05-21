# AWS Elastic Beanstalk

## What you'll learn
- What Elastic Beanstalk is and what it manages for you
- What platforms it supports
- When to use Beanstalk vs EC2 directly
- Key exam facts

---

## What is AWS Elastic Beanstalk?

AWS Elastic Beanstalk is a **Platform as a Service (PaaS)** that deploys and manages web applications and services. You upload your application code; Beanstalk automatically handles the deployment details — provisioning EC2 instances, configuring load balancers, setting up Auto Scaling, and monitoring health.

You retain full control of the underlying AWS resources and can access them directly at any time, but Beanstalk manages the provisioning and operations for you.

---

## Supported platforms

Elastic Beanstalk supports applications built with:
- Java (Tomcat)
- Node.js
- Python
- Ruby
- PHP
- .NET (Windows Server IIS)
- Go
- Docker (single and multi-container)

---

## What Beanstalk manages for you

When you deploy to Elastic Beanstalk, it automatically creates and configures:

| Component | What Beanstalk handles |
|-----------|----------------------|
| EC2 instances | Launches and configures the right instance type |
| Auto Scaling | Scales instances up/down based on load |
| Load balancer | Provisions an Application Load Balancer |
| Health monitoring | Reports application health, restarts unhealthy instances |
| Deployment | Supports rolling, blue/green, and all-at-once deployments |

You are only responsible for: your application code and the application-level configuration.

---

## When to use it

**Developers who want to focus on code** — A developer uploads their Python Flask app and Beanstalk handles all the infrastructure configuration.

**Standard web applications** — HTTP/HTTPS web servers and REST APIs that fit into one of the supported platform types.

**Quick prototyping** — Get an application running quickly without deep AWS infrastructure knowledge.

---

## Elastic Beanstalk vs EC2 directly

| | Elastic Beanstalk | EC2 (manual) |
|-|-------------------|--------------|
| Control | Less — Beanstalk manages infrastructure | Full — you configure everything |
| Speed to deploy | Fast — upload code and go | Slower — requires configuring all components |
| Flexibility | Limited to supported platforms | Any OS, any software |
| Best for | Standard web apps, less infrastructure expertise | Custom stacks, full OS control |

---

## Exam focus

- Beanstalk = **PaaS** — you provide the code; AWS handles the infrastructure
- Supports standard web frameworks and Docker
- **No additional cost** — you pay only for the underlying EC2, ELB, and other resources
- You can still access and modify the underlying resources directly
- Key differentiator from Lambda: Beanstalk runs traditional web servers on EC2; Lambda runs event-driven functions
- Use when the exam describes: "deploy web app without managing servers," "PaaS," "upload code and run"

---

## Practice questions

**Q1.** A team of developers wants to deploy a Node.js web application to AWS without learning how to configure EC2 instances, load balancers, and Auto Scaling. They just want to upload their code and have it run. Which service is most appropriate?

A) Amazon EC2  
B) AWS Elastic Beanstalk  
C) AWS Lambda  
D) Amazon ECS

**Answer: B** — Elastic Beanstalk is a PaaS designed exactly for this: developers upload application code and Beanstalk handles all the infrastructure. EC2 requires manual configuration. Lambda is for short, event-driven functions, not for deploying a traditional web server. ECS requires managing container configurations.
