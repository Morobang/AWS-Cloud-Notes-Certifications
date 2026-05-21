# AWS Compute Optimizer

## What you'll learn
- What Compute Optimizer does
- Which resources it analyzes
- How it differs from Trusted Advisor and Cost Explorer
- Key exam facts

---

## What is AWS Compute Optimizer?

AWS Compute Optimizer is a service that **analyzes your resource utilization metrics and recommends optimal AWS resource configurations** to reduce cost and improve performance. It uses machine learning to identify over-provisioned or under-provisioned resources.

The core problem it solves: many teams provision EC2 instances that are too large (over-provisioned), paying for CPU and memory that isn't used. Compute Optimizer identifies these and recommends right-sized alternatives.

---

## Resources Compute Optimizer analyzes

| Resource | What it recommends |
|----------|-------------------|
| EC2 instances | Right-sized instance type (e.g., downsize from m5.2xlarge to m5.large) |
| EC2 Auto Scaling Groups | Optimal instance types for the group |
| EBS volumes | Right-sized volume type and size |
| Lambda functions | Optimal memory allocation |
| ECS on Fargate | CPU and memory settings |

---

## How it works

1. Compute Optimizer collects CloudWatch metrics for your resources (at least 14 days of data; 3 months for better accuracy)
2. Its ML models analyze usage patterns — peak CPU, memory utilization, I/O patterns
3. It generates findings: **Over-provisioned**, **Under-provisioned**, **Optimized**, or **Not enough data**
4. For over-provisioned resources, it recommends specific alternative configurations

---

## Compute Optimizer vs Trusted Advisor vs Cost Explorer

| | Compute Optimizer | Trusted Advisor | Cost Explorer |
|-|------------------|-----------------|---------------|
| Focus | Right-sizing specific compute resources | Broad best practice checks (5 categories) | Historical cost analysis and trends |
| Detail | Specific instance type recommendations | High-level flags | Cost by service, account, tag |
| Trigger | Over/under-provisioned resources | Security, cost, performance checks | Spending anomalies |

---

## When to use it

**Right-size EC2 fleet** — Identify instances running at 5% CPU and recommend a smaller, cheaper instance type.

**Optimize Lambda memory** — Lambda functions charged by memory × duration; Compute Optimizer finds the memory setting that balances performance and cost.

**Reduce EBS costs** — Identify oversized EBS volumes that can be downsized or converted to a cheaper type.

---

## Exam focus

- Compute Optimizer = **ML-powered resource right-sizing recommendations**
- Analyzes: EC2, Auto Scaling Groups, EBS, Lambda, ECS/Fargate
- Identifies **over-provisioned** resources to reduce cost
- Uses **CloudWatch metrics** as input (needs 14+ days of data)
- Use when exam describes: "right-size EC2," "reduce costs by optimizing resource sizing," "ML recommendations for instance types"

---

## Practice questions

**Q1.** A company has a large EC2 fleet and suspects many instances are over-provisioned — running at low CPU utilization with more memory than needed. Which AWS service uses machine learning to analyze utilization data and recommend right-sized instance types?

A) AWS Cost Explorer  
B) AWS Trusted Advisor  
C) AWS Compute Optimizer  
D) Amazon CloudWatch

**Answer: C** — Compute Optimizer analyzes CloudWatch utilization metrics using ML and recommends specific right-sized instance types. Cost Explorer shows historical spend but doesn't recommend specific instance types. Trusted Advisor flags low-utilization EC2 instances at a high level but doesn't provide specific right-sizing recommendations. CloudWatch collects the metrics but doesn't make recommendations.
