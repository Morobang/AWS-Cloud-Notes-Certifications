# Domain 3: Cloud Technology and Services — Practice Questions

> 30 questions | ~34% of the CLF-C02 exam (most questions)

---

**Q1.** A company needs virtual machines with full control over the operating system and the ability to install custom software. Which AWS service should they use?

A) AWS Lambda  
B) Amazon EC2  
C) AWS Elastic Beanstalk  
D) Amazon ECS

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Amazon EC2 provides virtual machines (instances) where you choose the OS, install software, configure networking, and have root/admin access. It's the IaaS foundation of AWS.

- A (Lambda) is serverless — you don't manage the server or OS at all.
- C (Elastic Beanstalk) manages EC2 for you — you just provide your code.
- D (ECS) runs containers, not traditional VMs.
</details>

---

**Q2.** A developer wants to run code in response to HTTP requests without managing any servers. The code runs for less than 30 seconds per request. What is the BEST service?

A) Amazon EC2 with Auto Scaling  
B) AWS Lambda  
C) Amazon ECS  
D) AWS Elastic Beanstalk

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

AWS Lambda is serverless compute — you upload code, define a trigger (HTTP request via API Gateway), and Lambda runs it automatically. You pay only for execution time. No servers to manage.

The key criteria: "no servers" + "event-driven" = Lambda.

- A is over-engineered and requires managing EC2 instances.
- C and D involve managing container/app infrastructure.
</details>

---

**Q3.** Which EC2 pricing model is MOST appropriate for a workload that can be interrupted and needs the lowest possible cost?

A) On-Demand Instances  
B) Reserved Instances  
C) Dedicated Hosts  
D) Spot Instances

<details>
<summary>Answer & Explanation</summary>

**Answer: D**

Spot Instances use spare AWS capacity and can be up to 90% cheaper than On-Demand. The catch: AWS can reclaim them with a 2-minute warning. They're perfect for fault-tolerant workloads like batch processing, data analysis, or training ML models — anything that can checkpoint and resume.

- A is baseline pricing with no discount.
- B requires a 1 or 3 year commitment.
- C is the most expensive option (dedicated physical hardware).
</details>

---

**Q4.** A startup has unpredictable traffic and cannot commit to 1 or 3 years of compute usage. Which pricing model should they use?

A) Reserved Instances (Standard)  
B) On-Demand Instances  
C) Spot Instances  
D) Dedicated Instances

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

On-Demand is perfect for unpredictable workloads or when you can't make long-term commitments. No upfront payment, no contract — pay per hour or second of usage.

- A requires a 1 or 3 year commitment with upfront payment.
- C is interruptible — not suitable if availability is important.
- D is dedicated hardware with no discount for unpredictability.
</details>

---

**Q5.** What is the purpose of Amazon S3 Intelligent-Tiering storage class?

A) It stores data in a single Availability Zone to reduce cost  
B) It automatically moves data between storage tiers based on access patterns  
C) It provides the lowest storage cost for data that is never accessed  
D) It stores data with no retrieval fees, regardless of access frequency

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

S3 Intelligent-Tiering monitors access patterns and automatically moves objects between frequent, infrequent, and archive access tiers. No retrieval fees for moving between tiers. Ideal when you don't know how often data will be accessed.

- A describes S3 One Zone-IA.
- C describes S3 Glacier Deep Archive.
- D is misleading — Intelligent-Tiering has no retrieval charges but does charge a per-object monitoring fee.
</details>

---

**Q6.** A company needs to migrate 80 TB of on-premises data to AWS. Their internet connection is too slow for online transfer. Which AWS service should they use?

A) AWS Direct Connect  
B) AWS DataSync  
C) AWS Snowball Edge  
D) Amazon Kinesis

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

AWS Snowball Edge is a physical device shipped to your location. You load your data onto it and ship it back to AWS. For large datasets where internet transfer would take weeks or months, Snow Family devices are the answer.

- A (Direct Connect) is a dedicated network connection — still limited by bandwidth and not suitable for 80TB migrations within a reasonable time.
- B (DataSync) transfers over the network — same bandwidth problem.
- D (Kinesis) is for real-time streaming data, not bulk migration.
</details>

---

**Q7.** A developer needs a simple, managed database that automatically scales and can handle millions of requests per second with single-digit millisecond latency. Which service fits best?

A) Amazon RDS  
B) Amazon Redshift  
C) Amazon DynamoDB  
D) Amazon ElastiCache

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

DynamoDB is a serverless NoSQL database that scales automatically and delivers consistent single-digit millisecond performance at any scale. No server management, no schema migrations.

- A (RDS) is a relational database that doesn't automatically scale horizontally — you provision capacity.
- B (Redshift) is a data warehouse for analytics, not transactional workloads.
- D (ElastiCache) is an in-memory cache (Redis/Memcached) that sits IN FRONT of a database, not a database itself.
</details>

---

**Q8.** A company wants to analyze data stored in S3 using standard SQL queries without loading the data into a separate database. Which service enables this?

A) Amazon Redshift  
B) Amazon Athena  
C) Amazon EMR  
D) AWS Glue

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Amazon Athena is a serverless query service that lets you run SQL directly against data in S3 (CSV, JSON, Parquet, ORC). No loading required, no servers to manage — pay per query.

- A (Redshift) requires you to load data into the data warehouse first.
- C (EMR) is for large-scale Hadoop/Spark jobs — heavier and more complex.
- D (Glue) is ETL — it prepares/transforms data, not queries it directly.
</details>

---

**Q9.** Which AWS service provides a managed relational database that is compatible with MySQL and PostgreSQL but delivers up to 5x MySQL performance?

A) Amazon RDS  
B) Amazon Aurora  
C) Amazon DynamoDB  
D) Amazon Redshift

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Amazon Aurora is AWS's cloud-optimized relational database engine. It's MySQL and PostgreSQL compatible but uses a custom distributed storage architecture that delivers 5x MySQL and 3x PostgreSQL performance. Auto-scales storage up to 128 TB.

- A (RDS) can run MySQL/PostgreSQL but doesn't have Aurora's performance improvements.
- C and D are not relational databases in the traditional sense.
</details>

---

**Q10.** A company needs to speed up their application's database queries for frequently accessed data. Which service acts as an in-memory cache to reduce database load?

A) Amazon DynamoDB Accelerator (DAX)  
B) Amazon RDS Read Replicas  
C) Amazon ElastiCache  
D) Amazon Redshift

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Amazon ElastiCache provides managed Redis or Memcached in-memory caches. Frequently read data is stored in memory (microsecond latency) so your application doesn't hit the database every time. Dramatically reduces database load.

- A (DAX) is specifically a cache for DynamoDB, not for general databases.
- B (Read Replicas) distribute read load to copies of the database — still reading from disk.
- D (Redshift) is a data warehouse, not a cache.
</details>

---

**Q11.** What is the purpose of Amazon CloudFront?

A) It provides serverless compute for running code  
B) It is a content delivery network (CDN) that caches content at Edge Locations closer to users  
C) It connects on-premises networks to AWS via a dedicated connection  
D) It distributes incoming application traffic across multiple targets

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

CloudFront is AWS's CDN. It caches copies of your content (images, videos, HTML, APIs) at 400+ Edge Locations worldwide. When a user requests content, CloudFront serves it from the nearest Edge Location — reducing latency and origin server load.

- C describes AWS Direct Connect.
- D describes Elastic Load Balancing.
</details>

---

**Q12.** A company's application receives variable traffic — very heavy during business hours and almost nothing at night. Which AWS compute feature automatically adds and removes EC2 instances based on demand?

A) Elastic Load Balancing  
B) AWS Auto Scaling  
C) AWS Elastic Beanstalk  
D) Amazon EC2 Reserved Instances

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

AWS Auto Scaling (specifically EC2 Auto Scaling) monitors metrics and automatically adjusts the number of running instances based on defined policies — scaling out when traffic increases and scaling in when it drops, maintaining performance while minimizing cost.

- A (ELB) distributes traffic across existing instances but doesn't add/remove them.
- C (Elastic Beanstalk) can use Auto Scaling but isn't itself the scaling feature.
- D (Reserved Instances) is a pricing model, not a scaling feature.
</details>

---

**Q13.** Which service provides a managed message queue that decouples the components of a distributed application?

A) Amazon SNS  
B) Amazon SQS  
C) Amazon EventBridge  
D) AWS Step Functions

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Amazon SQS (Simple Queue Service) is a message queue. Producers send messages to the queue; consumers poll and process them at their own pace. This decouples services so a spike in orders doesn't overwhelm the fulfillment service — messages just queue up.

- A (SNS) is a pub/sub notification service — push model, multiple subscribers.
- C (EventBridge) routes events between services based on rules.
- D (Step Functions) orchestrates multi-step workflows.
</details>

---

**Q14.** What is the difference between Amazon SQS and Amazon SNS?

A) SQS is for sending emails; SNS is for sending SMS messages  
B) SQS is a pull-based queue for one-to-one messaging; SNS is a push-based pub/sub for one-to-many messaging  
C) SQS is for serverless applications only; SNS works with any AWS service  
D) SNS stores messages for long periods; SQS deletes them immediately

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

SQS = consumers PULL messages from a queue (point-to-point, one consumer per message).  
SNS = SNS PUSHES messages to all subscribers simultaneously (fan-out, many consumers per message).

The classic pattern: SNS topic → multiple SQS queues = fan-out. One event gets processed by multiple services independently.
</details>

---

**Q15.** A company wants to define their entire AWS infrastructure as code using templates, so they can recreate identical environments consistently. Which service should they use?

A) AWS Systems Manager  
B) AWS CloudFormation  
C) AWS Config  
D) AWS CodePipeline

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

AWS CloudFormation is Infrastructure as Code (IaC). You write YAML or JSON templates describing your resources, and CloudFormation creates/updates/deletes them. Identical templates = identical environments — great for dev/staging/production parity.

- A (Systems Manager) manages running instances (patching, run commands) — not IaC.
- C (Config) tracks configuration state — not IaC provisioning.
- D (CodePipeline) is a CI/CD orchestration tool — it might deploy CloudFormation templates but isn't IaC itself.
</details>

---

**Q16.** A financial services company needs to connect their on-premises data center to AWS with a consistent, high-bandwidth, low-latency private connection (not over the public internet). Which service should they use?

A) AWS VPN  
B) Amazon CloudFront  
C) AWS Direct Connect  
D) Amazon Route 53

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

AWS Direct Connect establishes a dedicated private network connection from your data center directly to AWS — not over the public internet. This provides consistent bandwidth, lower latency, and often lower data transfer costs for high-volume workloads.

- A (VPN) goes over the public internet (encrypted) — inconsistent and lower bandwidth.
- B (CloudFront) is a CDN, not a private network connection.
- D (Route 53) is DNS.
</details>

---

**Q17.** Which service provides DNS (Domain Name System) management and domain registration on AWS?

A) Amazon CloudFront  
B) AWS Direct Connect  
C) Amazon Route 53  
D) Amazon VPC

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Amazon Route 53 is AWS's DNS service. It translates domain names (like myapp.com) into IP addresses. It also supports health checks, traffic routing policies (latency-based, failover, geolocation), and domain registration.

The name "Route 53" comes from port 53, which is the standard DNS port.
</details>

---

**Q18.** A company needs to ensure their application is highly available across multiple Availability Zones and needs traffic distributed evenly across healthy instances. Which combination of services should they use?

A) Amazon CloudFront + Amazon Route 53  
B) Elastic Load Balancing + AWS Auto Scaling  
C) AWS Direct Connect + Amazon VPC  
D) Amazon SNS + Amazon SQS

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Elastic Load Balancing (ELB) distributes incoming traffic across multiple EC2 instances in different AZs and routes only to healthy instances. Auto Scaling ensures the right number of instances are running. Together they provide high availability and elasticity.

- A is for CDN and DNS, not application availability.
- C is for network connectivity.
- D is for messaging and decoupling.
</details>

---

**Q19.** A company uses Amazon S3 to store important business documents. They want to protect against accidental deletion. Which S3 feature should they enable?

A) S3 Transfer Acceleration  
B) S3 Versioning  
C) S3 Lifecycle Policies  
D) S3 Cross-Region Replication

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

S3 Versioning keeps all versions of an object, even after deletion (a delete operation just creates a "delete marker"). You can restore any previous version. This protects against both accidental deletion and overwrites.

- A (Transfer Acceleration) speeds up uploads using CloudFront Edge Locations.
- C (Lifecycle Policies) automatically transitions/expires objects based on age.
- D (Cross-Region Replication) copies objects to another region — provides DR but doesn't specifically protect against accidental deletion (the deletion would replicate too).
</details>

---

**Q20.** Which AWS service would you use to recognize objects, people, text, and scenes in images and videos automatically using machine learning?

A) Amazon Textract  
B) Amazon Comprehend  
C) Amazon Rekognition  
D) Amazon Polly

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Amazon Rekognition uses deep learning to analyze images and videos — detecting objects, faces, celebrities, text in images, unsafe content, and activities. No ML expertise required.

- A (Textract) extracts text and data from documents (forms, tables).
- B (Comprehend) does NLP on text (sentiment, entities, key phrases).
- D (Polly) converts text to lifelike speech.
</details>

---

**Q21.** A company wants to convert customer support calls from audio to text automatically. Which AWS service provides this?

A) Amazon Polly  
B) Amazon Transcribe  
C) Amazon Lex  
D) Amazon Translate

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Amazon Transcribe is the speech-to-text service. It converts audio to text, supports multiple languages, and can identify different speakers. Used for transcribing call center recordings, meetings, etc.

- A (Polly) does the opposite — TEXT to speech.
- C (Lex) builds conversational chatbots/voice interfaces.
- D (Translate) translates text between languages.
</details>

---

**Q22.** Which service provides automated recommendations to help reduce costs, improve performance, and address security gaps in your AWS environment?

A) AWS Config  
B) Amazon CloudWatch  
C) AWS Trusted Advisor  
D) AWS Cost Explorer

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

AWS Trusted Advisor analyzes your AWS environment across five categories: Cost Optimization, Performance, Security, Fault Tolerance, and Service Limits. It gives specific, actionable recommendations like "You have EC2 instances with no incoming traffic — consider terminating them."

- A (Config) evaluates compliance but doesn't give cost/performance optimization advice.
- B (CloudWatch) monitors metrics and alarms but doesn't give optimization recommendations.
- D (Cost Explorer) visualizes spending data but doesn't give the same breadth of recommendations.
</details>

---

**Q23.** What is Amazon VPC used for?

A) Speeding up data transfer between AWS Regions  
B) Creating a logically isolated private network in AWS where you control IP ranges, subnets, routing, and security  
C) Monitoring the performance of AWS services  
D) Providing serverless compute for running code in response to events

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Amazon VPC (Virtual Private Cloud) is your isolated network space in AWS. You define IP address ranges (CIDR), create subnets, configure route tables, and control inbound/outbound traffic with security groups and NACLs. It's like having your own private section of the AWS network.

All other options describe completely different services.
</details>

---

**Q24.** What is the difference between a Security Group and a Network Access Control List (NACL) in AWS?

A) Security Groups are for S3; NACLs are for EC2  
B) Security Groups are stateful instance-level firewalls; NACLs are stateless subnet-level firewalls  
C) Security Groups block traffic by default; NACLs allow all traffic by default  
D) Security Groups and NACLs perform identical functions

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Security Groups = stateful (return traffic automatically allowed), applied to instances.  
NACLs = stateless (must explicitly allow both directions), applied to subnets.

Default behavior:
- New Security Group: blocks all inbound, allows all outbound.
- Default NACL: allows all inbound and outbound (new custom NACLs deny all by default).
</details>

---

**Q25.** A company wants to run Docker containers on AWS without managing the underlying servers. Which service provides this capability?

A) Amazon EC2  
B) Amazon ECS with Fargate  
C) AWS Elastic Beanstalk  
D) Amazon Lightsail

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

AWS Fargate is a serverless compute engine for containers. When used with ECS (or EKS), you define your containers and Fargate runs them — you don't provision or manage EC2 instances. Pay per vCPU and memory used.

- A (EC2) is VMs — you manage the server.
- C (Elastic Beanstalk) can run containers but still provisions EC2 underneath.
- D (Lightsail) is simplified VMs/containers but isn't the designated serverless container solution.
</details>

---

**Q26.** Which AWS service helps you deploy and manage containerized applications using Kubernetes?

A) Amazon ECS  
B) Amazon EKS  
C) AWS Fargate  
D) AWS Lambda

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Amazon EKS (Elastic Kubernetes Service) is the managed Kubernetes service on AWS. AWS manages the Kubernetes control plane; you manage the worker nodes (or use Fargate for serverless nodes).

- A (ECS) is AWS's own container orchestration system (not Kubernetes).
- C (Fargate) is the compute engine for containers — it works with BOTH ECS and EKS.
- D (Lambda) is serverless functions, not containers.
</details>

---

**Q27.** Which AWS service is best described as a "data warehouse" designed for running complex analytical queries on large datasets?

A) Amazon RDS  
B) Amazon DynamoDB  
C) Amazon Redshift  
D) Amazon Aurora

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Amazon Redshift is a petabyte-scale data warehouse optimized for OLAP (Online Analytical Processing) — complex queries across huge datasets. It uses columnar storage and parallel processing. Used for BI and historical reporting.

- A and D are OLTP (transactional) relational databases.
- B is a NoSQL database, not a data warehouse.

Memory trick: **Red**shift = **reporting/BI/analytics**, RDS = **regular transactional apps**.
</details>

---

**Q28.** A company needs to process a stream of clickstream data from their website in real time. Which service should they use?

A) Amazon SQS  
B) Amazon Kinesis Data Streams  
C) AWS Glue  
D) Amazon Athena

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Amazon Kinesis is built for real-time streaming data. Kinesis Data Streams ingests large streams of data (clicks, IoT sensor data, logs) continuously. Other Kinesis services process and analyze it in real time.

- A (SQS) is a message queue for decoupling — not optimized for high-throughput streaming analytics.
- C (Glue) is batch ETL, not real-time streaming.
- D (Athena) queries historical data in S3, not real-time streams.
</details>

---

**Q29.** Which service provides logs, metrics, dashboards, and alarms to monitor the health and performance of AWS resources?

A) AWS CloudTrail  
B) Amazon CloudWatch  
C) AWS X-Ray  
D) AWS Config

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Amazon CloudWatch is the observability platform for AWS. It collects metrics (CPU usage, request count), logs (application logs, Lambda logs), creates dashboards, and triggers alarms when thresholds are breached.

- A (CloudTrail) records API calls for audit — who did what, not performance metrics.
- C (X-Ray) traces requests through distributed applications to identify bottlenecks.
- D (Config) tracks configuration changes, not performance.
</details>

---

**Q30.** What does AWS Elastic Beanstalk do?

A) Provides a message queuing service for decoupling applications  
B) Deploys and manages web applications automatically — you provide the code, Beanstalk handles the infrastructure  
C) Provides a content delivery network for static assets  
D) Monitors AWS resources and sends alerts

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Elastic Beanstalk is a PaaS (Platform as a Service). You upload your application code (in Python, Node.js, Java, PHP, etc.) and Beanstalk automatically provisions EC2, Load Balancers, Auto Scaling, and RDS — you don't configure any of that. Ideal for developers who want to deploy without learning AWS infrastructure.

You still have access to the underlying resources if needed — Beanstalk is a convenience layer, not a lock-in.
</details>
