# Domain 4: Billing, Pricing & Support — Practice Questions

> 15 questions | ~12% of the CLF-C02 exam

---

**Q1.** A company is planning to migrate to AWS and wants to estimate their monthly costs before committing. Which tool should they use?

A) AWS Cost Explorer  
B) AWS Budgets  
C) AWS Pricing Calculator  
D) AWS Cost and Usage Report

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

AWS Pricing Calculator (calculator.aws) lets you model your expected infrastructure and estimate costs BEFORE deploying anything. You add services, configure them, and get a monthly estimate.

- A (Cost Explorer) analyzes PAST spending — you need to already be using AWS.
- B (Budgets) sets ALERTS on spending — again, requires active usage.
- D (CUR) exports detailed billing data — also requires active usage.
</details>

---

**Q2.** Which AWS service allows you to set a spending threshold and receive an alert when your AWS costs or usage exceed that threshold?

A) AWS Cost Explorer  
B) AWS Cost and Usage Reports  
C) AWS Budgets  
D) AWS Pricing Calculator

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

AWS Budgets lets you define budgets for cost, usage, or Reserved Instance/Savings Plan utilization, and sends alerts via email or SNS when you approach or exceed the threshold. Essential for preventing surprise bills.

- A (Cost Explorer) shows historical spending visualizations — no alerting.
- B (CUR) gives detailed billing data exports — no alerting built in.
- D (Pricing Calculator) estimates future costs before deployment.
</details>

---

**Q3.** What are the three fundamental cost drivers in AWS pricing? (Select THREE)

A) Number of IAM users  
B) Compute resources used  
C) Number of AWS Regions enabled  
D) Data stored  
E) Data transferred out of AWS

<details>
<summary>Answer & Explanation</summary>

**Answers: B, D, E**

The three things you pay for in AWS:
1. **Compute** — time your resources run (EC2 hours, Lambda invocations)
2. **Storage** — amount of data stored (S3 GB, EBS GB)
3. **Data transfer OUT** — data leaving AWS to the internet (inbound is always free)

- A (IAM users) — IAM is always free.
- C (Regions enabled) — no charge for enabling Regions.
</details>

---

**Q4.** A company has multiple AWS accounts for different departments. They want a single bill and want to benefit from combined volume discounts. Which feature provides this?

A) AWS Organizations with Consolidated Billing  
B) AWS Cost Explorer  
C) AWS Budgets  
D) AWS Marketplace

<details>
<summary>Answer & Explanation</summary>

**Answer: A**

Consolidated Billing in AWS Organizations combines usage across all member accounts into a single bill. This means if Department A uses 5TB of S3 and Department B uses 5TB, the organization sees 10TB total — qualifying for the next volume discount tier. Savings are redistributed automatically.

All other options are cost management/visibility tools, not billing aggregation.
</details>

---

**Q5.** Which AWS support plan is required to get 24/7 phone and chat access to AWS Support engineers?

A) Basic  
B) Developer  
C) Business  
D) Enterprise On-Ramp

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Business is the minimum plan that includes 24/7 access to Cloud Support Engineers via phone, chat, and email. 

- Basic = no paid support, just documentation and forums.
- Developer = business hours email only, one contact.
- Business+ = 24/7 multi-channel support.
</details>

---

**Q6.** A large enterprise needs a dedicated Technical Account Manager (TAM) who proactively monitors their AWS environment and helps with architecture guidance. Which support plan includes a TAM?

A) Business  
B) Developer  
C) Enterprise On-Ramp  
D) Basic

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Both Enterprise On-Ramp and Enterprise plans include TAMs. Enterprise On-Ramp gives access to a **pool** of TAMs. The full Enterprise plan gives a **dedicated** TAM.

Business plan has access to AWS Support engineers but no dedicated TAM.
</details>

---

**Q7.** Which of the following is always free under the AWS Free Tier? (Select TWO)

A) 750 hours of EC2 t2.micro per month (first 12 months)  
B) AWS IAM  
C) Amazon S3 Standard storage (unlimited)  
D) 1 million AWS Lambda requests per month  
E) Amazon RDS (first 12 months)

<details>
<summary>Answer & Explanation</summary>

**Answers: B and D**

- **IAM** is always free — no charge for creating users, groups, roles, or policies.
- **Lambda free tier** (1M requests + 400,000 GB-seconds of compute) is **always free**, not just for 12 months.

- A (EC2 t2.micro) is free for 12 months only, then charges apply.
- C (S3) is not unlimited free — there's a 5GB limit for 12 months.
- E (RDS) free tier is for 12 months only.
</details>

---

**Q8.** Which AWS tool provides the MOST detailed breakdown of AWS costs and can be exported to S3 for analysis with tools like Athena or QuickSight?

A) AWS Cost Explorer  
B) AWS Budgets  
C) AWS Cost and Usage Report (CUR)  
D) AWS Pricing Calculator

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

The AWS Cost and Usage Report (CUR) is the most comprehensive billing dataset. It contains every line item of cost including resource IDs, usage types, tags, and pricing details. It can be exported to S3 and queried with Athena for custom analysis.

- A (Cost Explorer) has good filtering and visualization but is less granular than CUR.
- B (Budgets) is for alerts, not detailed analysis.
- D (Pricing Calculator) is for estimates before deployment.
</details>

---

**Q9.** A company has committed to using a specific amount of compute in exchange for a discount over 1 or 3 years, but they want flexibility to change instance types, OS, and regions. Which pricing option fits?

A) Standard Reserved Instances  
B) Convertible Reserved Instances  
C) Savings Plans  
D) Spot Instances

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Compute Savings Plans give discounts (up to 66%) in exchange for a commitment to a dollar amount of compute usage per hour for 1 or 3 years — but with flexibility to change instance family, region, OS, and tenancy. More flexible than Reserved Instances.

- A (Standard Reserved) locks you to a specific instance type and region.
- B (Convertible Reserved) allows some changes but with less discount than Standard Reserved.
- D (Spot) is for interruptible workloads, not committed steady-state workloads.
</details>

---

**Q10.** Which of the following correctly describes what the AWS Free Tier "12-Month Free" category means?

A) Services that are free forever as long as usage stays below defined limits  
B) Services that are free for the first 12 months after your AWS account creation  
C) Services that offer a one-time 12-month discount after contacting AWS Sales  
D) Services that are free only for the first 12 resources created

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

The 12-Month Free tier includes services like EC2 (750 hours of t2.micro/month), S3 (5GB), and RDS (750 hours) — free for 12 months from the date you create your AWS account. After 12 months, standard pricing applies.

The three Free Tier types:
- **Always Free** — no expiry (IAM, Lambda 1M requests, DynamoDB 25GB)
- **12 Months Free** — 12 months from account creation
- **Trials** — specific time-limited trials of services
</details>

---

**Q11.** A startup is on the Basic support plan and encounters a production outage. What support options are available to them?

A) 24/7 phone support with a 1-hour response time  
B) Access to AWS documentation, whitepapers, and community forums only  
C) Email support within 4 business hours  
D) A dedicated TAM who will resolve the issue

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

The Basic (free) support plan provides access to documentation, AWS Trusted Advisor (7 checks), the AWS Health Dashboard, and community forums. No paid engineer support of any kind.

This is why it's critical for production workloads to be on at least the Business plan.
</details>

---

**Q12.** Which AWS service helps identify underutilized resources and provides recommendations to rightsize EC2 instances to reduce cost?

A) AWS Budgets  
B) AWS Cost Explorer  
C) AWS Compute Optimizer  
D) AWS Trusted Advisor

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

AWS Compute Optimizer uses ML to analyze EC2 usage metrics and recommends optimal instance types — for example, suggesting you downgrade from a m5.xlarge to a m5.large if your CPU usage never exceeds 20%.

- B (Cost Explorer) shows spending patterns but doesn't give specific rightsizing recommendations.
- D (Trusted Advisor) also has some cost optimization checks but Compute Optimizer is the dedicated rightsizing tool.
</details>

---

**Q13.** Data transfer INTO AWS (ingress) is priced at what rate?

A) The same rate as data transfer out  
B) Double the rate of data transfer out  
C) Free  
D) Based on the originating country

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

AWS charges nothing for data transferred INTO AWS from the internet (inbound/ingress). You only pay for data going OUT (egress). This is by design — AWS wants you to bring your data in.

This is an exam favorite. Always remember: **Inbound = Free, Outbound = Pay.**
</details>

---

**Q14.** A company uses AWS and wants to understand their spending at a granular level — which team is spending what, on which services. Which approach enables this cost allocation?

A) Enable AWS CloudTrail in all regions  
B) Create separate AWS accounts for each team  
C) Apply cost allocation tags to AWS resources  
D) Use AWS Config to track resource usage

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Cost allocation tags let you tag resources with metadata like `Team: Marketing`, `Project: WebApp`, `Environment: Production`. You then activate those tags in Billing and can filter Cost Explorer or CUR reports by tag to see exactly which team or project generated which costs.

- B works but is overkill — you'd still want tags within each account.
- A and D don't help with cost attribution.
</details>

---

**Q15.** What is the main purpose of AWS Marketplace?

A) A place where AWS provides pricing discounts for Reserved Instances  
B) A digital catalog where customers can find, buy, and deploy third-party software that runs on AWS  
C) An internal AWS store for purchasing hardware devices  
D) A marketplace for selling unused EC2 Reserved Instances

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

AWS Marketplace is a curated digital catalog with thousands of software listings from independent vendors — security tools, ML models, databases, operating systems, developer tools. You can deploy them with a few clicks and pay through your AWS bill.

- D describes the Reserved Instance Marketplace (a separate, specific feature).
</details>
