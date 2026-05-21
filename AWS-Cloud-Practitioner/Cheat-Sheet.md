# AWS Cloud Practitioner — Cheat Sheet

> One-page reference for the CLF-C02 exam. All 4 domains, key services, important numbers, and exam traps.

---

## DOMAIN 1 — Cloud Concepts (24%)

### Six Benefits of Cloud Computing
| Benefit | One-liner |
|---|---|
| Trade fixed for variable expense | Pay-as-you-go instead of upfront hardware costs |
| Massive economies of scale | AWS buys in bulk → passes savings to you |
| Stop guessing capacity | Scale up/down on demand, no over/under provisioning |
| Increase speed and agility | Resources in minutes, not weeks |
| Stop spending on data centers | Focus on your product, not facilities |
| Go global in minutes | Deploy to any AWS Region with a few clicks |

### Cloud Deployment Models
| Model | What it means | Example |
|---|---|---|
| **Public Cloud** | Everything on AWS | Netflix on AWS |
| **Private Cloud** | Your own cloud infrastructure | Bank's internal data center |
| **Hybrid** | Mix of on-premises + AWS | Core systems on-prem, burst to AWS |

### Cloud Service Models
| Model | AWS manages | You manage | Example |
|---|---|---|---|
| **IaaS** | Hardware, network | OS, apps, data | EC2 |
| **PaaS** | Hardware + OS | Apps, data | Elastic Beanstalk |
| **SaaS** | Everything | Just use it | Amazon WorkMail |

### Well-Architected Framework — 6 Pillars
1. **Operational Excellence** — run and improve systems
2. **Security** — protect data and systems
3. **Reliability** — recover from failures automatically
4. **Performance Efficiency** — use resources efficiently
5. **Cost Optimization** — eliminate waste
6. **Sustainability** — minimize environmental impact

### AWS Cloud Adoption Framework (CAF) — 6 Perspectives
**Business side**: Business, People, Governance  
**Technical side**: Platform, Security, Operations

### Migration Strategies — 7 Rs
| Strategy | Meaning |
|---|---|
| Retire | Shut it down, no longer needed |
| Retain | Keep on-premises for now |
| Rehost | "Lift and shift" to AWS with no changes |
| Relocate | Move to AWS at hypervisor level (VMware) |
| Repurchase | Replace with SaaS (e.g., move CRM to Salesforce) |
| Replatform | Minor optimizations, no code changes |
| Refactor/Re-architect | Redesign using cloud-native features |

---

## DOMAIN 2 — Security and Compliance (30%)

### Shared Responsibility Model
| AWS Responsible FOR | Customer Responsible FOR |
|---|---|
| Physical hardware/facilities | Customer data |
| Global network infrastructure | Identity and access management (IAM) |
| Hypervisor/virtualization layer | OS patches on EC2 |
| Managed service security (RDS OS patches) | Application security |
| "Security OF the cloud" | "Security IN the cloud" |

**Memory trick**: If YOU can configure it → it's YOUR responsibility.

### IAM Key Concepts
| Concept | What it is |
|---|---|
| **Root user** | God mode — only for billing setup, never for daily use |
| **IAM User** | A person or service with credentials |
| **IAM Group** | Collection of users sharing the same permissions |
| **IAM Role** | Temporary identity assumed by services or users |
| **IAM Policy** | JSON document that defines what actions are allowed/denied |
| **MFA** | Multi-Factor Authentication — required for root, recommended for all |

### Principle of Least Privilege
Grant only the minimum permissions needed to do the job. Nothing more.

### Key Security Services
| Service | What it does |
|---|---|
| **AWS IAM** | Manage users, roles, permissions |
| **AWS Organizations** | Manage multiple AWS accounts centrally |
| **AWS Control Tower** | Set up and govern multi-account environments |
| **AWS Shield** | DDoS protection (Standard = free, Advanced = paid) |
| **AWS WAF** | Web Application Firewall — blocks bad HTTP traffic |
| **Amazon GuardDuty** | Threat detection using ML on logs |
| **Amazon Inspector** | Automated vulnerability scanning for EC2/containers |
| **Amazon Macie** | Finds and protects sensitive data (PII) in S3 |
| **AWS Security Hub** | Central security dashboard |
| **AWS CloudTrail** | Records every API call (who did what, when) |
| **AWS Config** | Tracks configuration changes over time |
| **AWS Artifact** | Access compliance reports and agreements |
| **AWS KMS** | Create and manage encryption keys |
| **AWS Secrets Manager** | Store and rotate secrets (passwords, API keys) |
| **Amazon Cognito** | User identity for web/mobile apps |

### Compliance Programs to Know
- **HIPAA** — US healthcare data
- **PCI DSS** — Payment card industry
- **SOC 1/2/3** — Service organization controls
- **ISO 27001** — Information security management
- **GDPR** — EU data protection

---

## DOMAIN 3 — Cloud Technology and Services (34%)

### Global Infrastructure
| Term | What it means |
|---|---|
| **Region** | Geographic area with 2+ Availability Zones (e.g., us-east-1) |
| **Availability Zone (AZ)** | One or more discrete data centers in a Region |
| **Edge Location** | CloudFront CDN cache point — closer to end users |
| **Local Zone** | AWS infrastructure placed in major cities for ultra-low latency |
| **Wavelength Zone** | AWS in telecom networks for 5G edge workloads |
| **AWS Outposts** | AWS hardware installed in YOUR data center |

**Key numbers**: 30+ Regions, 90+ AZs, 400+ Edge Locations (as of 2024)

### Compute Services
| Service | What it does | When to use |
|---|---|---|
| **Amazon EC2** | Virtual machines in the cloud | Full control over OS, custom workloads |
| **AWS Lambda** | Run code without servers | Event-driven, short tasks (<15 min) |
| **AWS Elastic Beanstalk** | Deploy apps without managing infrastructure | Developers who want simplicity |
| **Amazon ECS** | Run Docker containers | Container workloads on managed infra |
| **Amazon EKS** | Managed Kubernetes | Kubernetes workloads |
| **AWS Fargate** | Serverless containers (works with ECS/EKS) | Containers without managing servers |
| **Amazon Lightsail** | Simple VPS — cheap, easy | Small websites, dev/test |
| **AWS Batch** | Run batch computing jobs | Large-scale data processing jobs |
| **AWS Outposts** | AWS rack in your data center | Low-latency on-prem workloads |

### EC2 Pricing Models
| Model | When to use | Savings vs On-Demand |
|---|---|---|
| **On-Demand** | Unpredictable, short-term | Baseline |
| **Reserved Instances** | Steady-state, 1 or 3 year commitment | Up to 72% |
| **Savings Plans** | Flexible commitment (not tied to instance type) | Up to 72% |
| **Spot Instances** | Fault-tolerant, interruptible workloads | Up to 90% |
| **Dedicated Hosts** | Compliance requires dedicated hardware | Expensive |
| **Dedicated Instances** | Your workloads on dedicated hardware | Expensive |

### Storage Services
| Service | Type | When to use |
|---|---|---|
| **Amazon S3** | Object storage | Files, backups, static websites, data lakes |
| **Amazon EBS** | Block storage (attached to EC2) | EC2 hard drive replacement |
| **Amazon EFS** | Elastic file system (shared) | Multiple EC2 sharing same file system |
| **Amazon Glacier / S3 Glacier** | Archival cold storage | Long-term backups, compliance archives |
| **AWS Storage Gateway** | Hybrid on-prem to S3 bridge | Connect on-prem to cloud storage |
| **AWS Snow Family** | Physical data transfer devices | Move petabytes of data offline |

### S3 Storage Classes
| Class | Use case | Retrieval |
|---|---|---|
| S3 Standard | Frequently accessed | Instant |
| S3 Intelligent-Tiering | Unknown access patterns | Instant |
| S3 Standard-IA | Infrequent access, rapid retrieval | Instant |
| S3 One Zone-IA | Infrequent, non-critical | Instant |
| S3 Glacier Instant Retrieval | Archive, millisecond access | Instant |
| S3 Glacier Flexible Retrieval | Archive, minutes–hours | Minutes to hours |
| S3 Glacier Deep Archive | Cheapest, rarely accessed | 12–48 hours |

### Database Services
| Service | Type | Best for |
|---|---|---|
| **Amazon RDS** | Managed relational DB | MySQL, PostgreSQL, Oracle, SQL Server |
| **Amazon Aurora** | AWS-optimized relational DB | High performance MySQL/PostgreSQL |
| **Amazon DynamoDB** | NoSQL key-value + document | Serverless, low-latency at scale |
| **Amazon ElastiCache** | In-memory cache | Redis/Memcached — speed up DB queries |
| **Amazon Redshift** | Data warehouse | Analytics, BI, large-scale SQL queries |
| **Amazon Neptune** | Graph database | Social networks, fraud detection graphs |
| **Amazon DocumentDB** | MongoDB-compatible document DB | Document workloads |

### Networking Services
| Service | What it does |
|---|---|
| **Amazon VPC** | Your private virtual network in AWS |
| **Subnet** | Subdivision of VPC (public or private) |
| **Security Group** | Virtual firewall at the instance level (stateful) |
| **Network ACL (NACL)** | Firewall at the subnet level (stateless) |
| **Internet Gateway** | Connect VPC to the internet |
| **NAT Gateway** | Let private subnet resources access internet (outbound only) |
| **Amazon Route 53** | DNS service + domain registration |
| **Amazon CloudFront** | CDN — cache content at Edge Locations |
| **AWS Direct Connect** | Dedicated private line from your data center to AWS |
| **AWS VPN** | Encrypted tunnel over the internet to AWS |
| **Elastic Load Balancer** | Distribute traffic across multiple targets |

**Security Group vs NACL**:
- Security Group = stateful (return traffic automatically allowed)
- NACL = stateless (must explicitly allow both directions)

### Analytics Services
| Service | What it does |
|---|---|
| **Amazon Athena** | Query S3 data with SQL (serverless) |
| **AWS Glue** | ETL — discover, prepare, and transform data |
| **Amazon Kinesis** | Real-time streaming data ingestion and processing |
| **Amazon EMR** | Managed Hadoop/Spark for big data |
| **Amazon Redshift** | Data warehouse for analytics |
| **Amazon QuickSight** | Business intelligence / dashboards |
| **Amazon OpenSearch** | Search and log analytics |

### ML / AI Services
| Service | What it does |
|---|---|
| **Amazon SageMaker** | Build, train, and deploy ML models |
| **Amazon Rekognition** | Image and video analysis |
| **Amazon Textract** | Extract text from documents (OCR+) |
| **Amazon Comprehend** | NLP — sentiment, entities, key phrases |
| **Amazon Transcribe** | Speech to text |
| **Amazon Polly** | Text to speech |
| **Amazon Translate** | Language translation |
| **Amazon Lex** | Build conversational chatbots |
| **Amazon Kendra** | Intelligent enterprise search |
| **Amazon Q** | AI assistant for business productivity |

### Application Integration
| Service | What it does |
|---|---|
| **Amazon SQS** | Message queue — decouple services |
| **Amazon SNS** | Pub/sub notifications — fan out messages |
| **Amazon EventBridge** | Event bus — route events between services |
| **AWS Step Functions** | Orchestrate multi-step workflows |

**SQS vs SNS**:
- SQS = one consumer pulls messages from a queue
- SNS = many subscribers get pushed the same message

### Management & Governance
| Service | What it does |
|---|---|
| **AWS CloudFormation** | Infrastructure as Code (IaC) — provision with templates |
| **AWS CloudTrail** | Audit log of every API call |
| **Amazon CloudWatch** | Metrics, logs, alarms, dashboards |
| **AWS Config** | Track and audit configuration changes |
| **AWS Systems Manager** | Manage EC2 at scale (patch, run commands) |
| **AWS Trusted Advisor** | Automated best-practice recommendations |
| **AWS Well-Architected Tool** | Review workloads against best practices |
| **AWS Health Dashboard** | Status of AWS services in your account |
| **AWS Organizations** | Manage multiple accounts, apply SCPs |
| **AWS Control Tower** | Landing zone setup for multi-account governance |
| **AWS Service Catalog** | Pre-approved product portfolios for your org |

### Developer Tools
| Service | What it does |
|---|---|
| **AWS CodePipeline** | CI/CD pipeline orchestration |
| **AWS CodeBuild** | Build and test code |
| **AWS CodeDeploy** | Deploy code to EC2, Lambda, on-prem |
| **AWS X-Ray** | Trace and debug distributed applications |
| **AWS CLI** | Command-line access to AWS services |

---

## DOMAIN 4 — Billing, Pricing & Support (12%)

### AWS Pricing Fundamentals
Three things you pay for:
1. **Compute** (time your resources run)
2. **Storage** (amount of data stored)
3. **Data transfer OUT** (data leaving AWS — inbound is free)

### Free Tier
| Type | What it means |
|---|---|
| **Always Free** | Never expires (e.g., 1M Lambda requests/month) |
| **12 Months Free** | Free for 12 months after signup (e.g., 750hrs EC2 t2.micro) |
| **Trials** | Short-term free use of specific services |

### Cost Management Tools
| Tool | What it does |
|---|---|
| **AWS Pricing Calculator** | Estimate costs before you deploy |
| **AWS Cost Explorer** | Visualize and analyze past spending |
| **AWS Budgets** | Set spending alerts — notify when threshold hit |
| **AWS Cost and Usage Report (CUR)** | Most detailed billing data, exports to S3 |
| **AWS Compute Optimizer** | Rightsizing recommendations |
| **AWS Marketplace** | Buy/sell third-party software on AWS |

### AWS Support Plans
| Plan | Price | Key Features |
|---|---|---|
| **Basic** | Free | Documentation, forums, Trusted Advisor (7 checks) |
| **Developer** | $29/month | Business hours email support, 1 contact |
| **Business** | $100/month | 24/7 phone/chat, full Trusted Advisor, AWS Support API |
| **Enterprise On-Ramp** | $5,500/month | Pool of TAMs, concierge support |
| **Enterprise** | $15,000/month | Dedicated TAM, concierge, fastest response times |

**TAM** = Technical Account Manager (only Enterprise plans)

### Response Time SLAs
| Severity | Business | Enterprise |
|---|---|---|
| General guidance | 24 business hours | 24 business hours |
| System impaired | 12 business hours | 12 business hours |
| Production impaired | 4 hours | 4 hours |
| Production down | 1 hour | 1 hour |
| Business-critical down | N/A | **15 minutes** |

### AWS Organizations & Consolidated Billing
- **Consolidated Billing**: One bill for all accounts, sharing volume discounts
- **Service Control Policies (SCPs)**: Guardrails applied to member accounts
- **Management Account**: The root/payer account
- **Member Account**: Any non-root account in the organization

---

## EXAM TRAPS & QUICK WINS

### Common Trick Questions
| Question trap | Right answer |
|---|---|
| "Which is AWS's responsibility?" | Physical hardware, managed service patches, hypervisor |
| "Which is the CUSTOMER's responsibility?" | IAM users/roles, EC2 OS patches, application data |
| "Cheapest for predictable 3-year workload?" | Reserved Instances |
| "Cheapest for fault-tolerant, flexible workload?" | Spot Instances |
| "Need phone support 24/7?" | Business plan or higher |
| "Need a dedicated TAM?" | Enterprise plan |
| "S3 cheapest storage?" | S3 Glacier Deep Archive |
| "Secure multi-account governance?" | AWS Organizations + Control Tower |
| "Who can close an AWS account?" | Root user only |
| "Audit trail of ALL API calls?" | AWS CloudTrail |
| "Real-time threat detection?" | Amazon GuardDuty |
| "Vulnerability scanning?" | Amazon Inspector |
| "Sensitive data discovery in S3?" | Amazon Macie |

### Key Numbers to Remember
- Lambda max timeout: **15 minutes**
- S3 object max size: **5 TB**
- S3 free tier: **5 GB** for 12 months
- EC2 free tier: **750 hours/month** of t2.micro for 12 months
- DynamoDB free tier: **25 GB** always free
- Basic Support: **7 Trusted Advisor checks**
- Business Support: **All Trusted Advisor checks**

### Service Name Patterns
- **Amazon** = consumer-facing / foundational services (EC2, S3, RDS)
- **AWS** = management / developer / platform tools (IAM, CloudFormation, CodePipeline)

---

*Exam: CLF-C02 | 65 questions | 90 minutes | Score 700/1000 to pass*
