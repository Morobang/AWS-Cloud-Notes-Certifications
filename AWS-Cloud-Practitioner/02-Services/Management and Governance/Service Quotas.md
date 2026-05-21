# Service Quotas

## What you'll learn
- What service quotas are
- How to view and request quota increases
- Key exam facts

---

## What are service quotas?

AWS imposes **limits on how many resources of each type you can use in a Region or account**. These limits are called service quotas (previously called service limits). They exist to protect AWS infrastructure from accidental misuse and to ensure fair resource availability.

Examples of service quotas:
- EC2: Maximum 32 vCPUs for On-Demand instances per Region (default for new accounts)
- Lambda: Maximum 1,000 concurrent executions per account per Region
- VPC: Maximum 5 VPCs per Region
- S3: Maximum 100 S3 buckets per account

---

## Why quotas exist

- **Prevent runaway costs**: An accidental loop that launches thousands of EC2 instances would hit the quota and stop
- **Resource fairness**: Prevents one account from monopolizing AWS infrastructure
- **Account validation**: New accounts start with conservative limits that increase as usage patterns are established

Most quotas can be increased by submitting a request to AWS Support.

---

## Service Quotas service

The **Service Quotas** service in the AWS console provides a central location to:

- **View** all your current quotas and applied limits across services
- **Request increases** directly from the console (for adjustable quotas) — no support ticket required for many services
- **Set CloudWatch alarms** on quota usage — get notified before you hit a limit
- **Compare** your current usage to your quota to see how close you are to the limit

---

## Adjustable vs fixed quotas

**Adjustable quotas**: Can be increased by requesting a quota increase. Examples: EC2 vCPU limits, Lambda concurrency limits, VPC count.

**Fixed quotas (hard limits)**: Cannot be increased regardless of request. Example: S3 has a maximum of 100 buckets per account that cannot be exceeded (though you can store unlimited objects within those buckets).

---

## How to request a quota increase

1. Open Service Quotas in the console
2. Find the service and quota you need increased
3. Click "Request quota increase"
4. Enter the desired value and business justification
5. AWS Support reviews and approves (usually within 1–2 business days)

For urgent needs, you can also open a support case directly.

---

## Exam focus

- Service Quotas = **manage and request increases to AWS service limits**
- All AWS services have default limits (quotas) per Region and account
- Most quotas are **adjustable** — request an increase through Service Quotas or Support
- **Some quotas are fixed** (hard limits) and cannot be changed
- Use when exam describes: "approaching EC2 instance limits," "request more Lambda concurrency," "view current service limits"

---

## Practice questions

**Q1.** A company is expanding their AWS usage and expects to need more than the default number of EC2 vCPUs in their account. What is the correct approach to increase this limit?

A) Create a new AWS account to get a fresh set of limits  
B) Contact AWS Support to request a service quota increase  
C) Limits cannot be changed — deploy workloads across multiple Regions  
D) Upgrade to a higher EC2 instance family

**Answer: B** — EC2 vCPU limits are adjustable quotas. The company can request an increase through the Service Quotas console or AWS Support. Creating a new account sidesteps the issue but doesn't solve it for the long term and adds complexity. Most limits are adjustable (not fixed). Instance families don't change the vCPU quota.
