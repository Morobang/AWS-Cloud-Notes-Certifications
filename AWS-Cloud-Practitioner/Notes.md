# AWS Cloud Practitioner — Personal Notes

> Personal takeaways, memory tricks, and "aha moments" from studying for CLF-C02.  
> These are the things that finally made things click.

---

## The mental model that unlocks everything

**AWS is a utility company.** You don't build a power plant to get electricity. You don't drill a well for water. You plug into the grid and pay for what you use. AWS is the same thing for computing.

Once that clicks, all the benefits (no upfront cost, scale on demand, pay-as-you-go) make instant sense.

---

## Domain 1: Cloud Concepts

### The six benefits — remember them as a story
You used to buy tons of hardware upfront (capex), run it at 20% capacity most of the time, guess badly about future needs, hire a team just to keep the lights on, and serve only local users. AWS flips every single one of those problems.

The one people forget: **"Stop spending on data centers"** — this isn't about cost, it's about where you focus. Infrastructure management is not your competitive advantage. Code is.

### Well-Architected Framework — the pillar I always forget
Everyone remembers Security, Reliability, Performance, Cost. The two people trip up on:
- **Operational Excellence** — not just "run stuff" but actively improve: automate runbooks, learn from failures, make small reversible changes
- **Sustainability** — added in 2021, it's about minimizing your carbon footprint on AWS

### The 7 Rs of migration — a trick to remember them
Think of a spectrum from zero effort to full rebuild:
```
Retire → Retain → Rehost → Relocate → Repurchase → Replatform → Refactor
```
Left side = lazy/cheap. Right side = effort/reward.

**Rehost vs Replatform**: Rehost = exactly the same, just on AWS hardware. Replatform = you make one or two tweaks to benefit from cloud (like moving to RDS instead of self-managed MySQL) but don't touch the application code.

### Cloud Adoption Framework (CAF)
I kept confusing the perspectives. Here's how I remembered them:

The **business side** (Business, People, Governance) is about convincing and enabling the HUMANS and the ORG to change.  
The **technical side** (Platform, Security, Operations) is about actually building and running the thing.

---

## Domain 2: Security and Compliance

### The shared responsibility model in plain English
The way I think about it: **AWS owns the building, you own what's inside your apartment.**

AWS built the building (physical servers, network cables, hypervisor). They maintain the structure. But what you put in your apartment (your data, your apps, your user accounts) — that's on you.

The confusing part: **managed services shift more responsibility to AWS.**
- EC2: You patch the OS yourself.
- RDS: AWS patches the database engine. You still manage the data inside.
- DynamoDB: AWS manages everything. You just manage your data and access.

As you move up the abstraction ladder, you own less of the infrastructure stack.

### IAM — the concepts that tripped me up

**Roles vs Users**: Users are for humans (or long-lived programmatic access). Roles are temporary — they're assumed by things that need permission for a limited time (an EC2 instance, a Lambda function, a user from another account). Think of a role like a visitor badge at an office — it's time-limited and purpose-specific.

**Policies attach to identities** (users/groups/roles). A user with no policies can log in but can't do anything. That's least privilege by default.

**The root user is basically a nuclear code.** Set MFA on it, put it in a safe, and never use it for day-to-day work. Create an admin IAM user and use that instead.

### Security services I kept mixing up
| Service | The one-liner I used to remember it |
|---|---|
| GuardDuty | The DETECTIVE — watches your logs and finds threats using ML |
| Inspector | The AUDITOR — scans your EC2/containers for vulnerabilities |
| Macie | The PRIVACY OFFICER — hunts for sensitive data (PII) in S3 |
| Shield | The BOUNCER — blocks DDoS attacks (Standard is free always) |
| WAF | The GATEKEEPER — filters bad web requests at the application layer |
| Config | The HISTORIAN — records every config change over time |
| CloudTrail | The LEDGER — records every API call ever made in your account |

**The CloudTrail vs CloudWatch confusion**:
- CloudTrail = WHO did WHAT to WHICH resource and WHEN (API calls)
- CloudWatch = HOW IS the resource performing right now (metrics, logs, alarms)

One is about accountability. The other is about observability.

---

## Domain 3: Cloud Technology and Services

### Global Infrastructure — what actually matters for the exam

**Regions are isolated from each other** — a disaster in us-east-1 doesn't affect eu-west-1.

**AZs are isolated within a region** — each AZ has its own power, cooling, networking. Deploying across 2+ AZs = high availability.

**Edge Locations are NOT AZs** — they're just caching points for CloudFront. They don't run your compute.

### Compute: the decision tree
```
Need full control of the OS?
  → EC2

Need to run code without managing servers?
  → Lambda (if < 15 min) or Fargate (containers)

Running containers?
  → ECS (simpler) or EKS (Kubernetes)

Just want to deploy an app and not think about infrastructure?
  → Elastic Beanstalk

Small website or dev environment on a budget?
  → Lightsail
```

### EC2 Pricing — the one that shows up most on the exam
The exam loves asking "what's the cheapest option for X workload?"

- **Steady, predictable workload for 1-3 years?** → Reserved Instances (up to 72% off)
- **Flexible commitment to compute in general?** → Savings Plans
- **Can handle being interrupted at any time?** → Spot Instances (up to 90% off)
- **Short-term, can't commit?** → On-Demand
- **Must be on dedicated physical hardware?** → Dedicated Hosts (compliance reasons)

The sneaky trap: **Spot Instances are cheapest but interruptible** — AWS can take them back with 2 minutes notice. Only use for things that can handle sudden stops (batch jobs, data analysis, not web servers).

### Storage: the ladder to understand S3 classes
```
Hot data (access daily)   → S3 Standard
Unknown access patterns   → S3 Intelligent-Tiering (auto-moves between tiers)
Access monthly maybe      → S3 Standard-IA or One Zone-IA
Archive (access in mins)  → S3 Glacier Instant Retrieval
Archive (can wait hours)  → S3 Glacier Flexible Retrieval
Archive (can wait days)   → S3 Glacier Deep Archive ← CHEAPEST
```

### Databases: relational vs NoSQL
**When the exam says "relational" or mentions SQL / tables / joins** → RDS or Aurora  
**When the exam says "serverless", "high scale", "key-value", "millisecond latency"** → DynamoDB  
**When the exam says "analytics", "BI", "historical data"** → Redshift (it's a data warehouse, not a regular database)

Aurora = RDS on steroids. 5x faster than MySQL, 3x faster than PostgreSQL. Auto-scales storage. More expensive but worth it for production.

### Networking: the two I always confused

**Security Groups** = door lock on each apartment (instance level, stateful)  
**NACLs** = security at the building entrance (subnet level, stateless)

Stateful = it remembers your request and lets the response back in automatically.  
Stateless = it forgets. You have to explicitly allow both the request AND the response direction.

### SQS vs SNS — the one that comes up constantly
```
SQS = a queue. One producer, one consumer. Consumer polls for messages. Good for: decoupling, rate limiting, async tasks.

SNS = a broadcast. One publisher, many subscribers. Push model. Good for: notifications, fan-out to multiple services.
```

Real-world analogy:
- SQS = a mailbox. One person checks it.
- SNS = a group chat notification. Everyone in the group gets it simultaneously.

Common exam pattern: SQS + SNS together = "fan-out pattern." SNS broadcasts to multiple SQS queues so each subscriber processes at its own pace.

---

## Domain 4: Billing, Pricing & Support

### The three things you pay for
1. Compute time (EC2 running, Lambda executions)
2. Storage amount (GB stored in S3, EBS, etc.)
3. Data transfer OUT (inbound is free, cross-AZ has a small cost)

**Data transfer OUT is the sneaky cost.** Download from S3 to the internet costs money. Uploading to S3 is free.

### Support plans — how I remembered the tiers
Think of it as what problem you're trying to solve:
- **Basic** = You're fine with documentation and forums.
- **Developer** = You want email support when you're stuck, but you're not running production yet.
- **Business** = You're in production and need someone to call at 3am.
- **Enterprise** = You have a dedicated person at AWS who knows your architecture.

The exam loves this question: "Which support plan includes a Technical Account Manager (TAM)?" → **Enterprise (and Enterprise On-Ramp)**

### Consolidated billing tip
If you have multiple AWS accounts under Organizations, you share volume discounts across ALL accounts. So if account A uses 5TB of S3 and account B uses 5TB, it looks like 10TB total → better pricing tier → savings.

---

## Things I Got Wrong in Practice Tests (and Why)

**Q: Who is responsible for patching the OS on an RDS instance?**  
A: AWS. With RDS, AWS manages the database engine and OS. You only manage the data and schemas.

**Q: What protects against DDoS for free?**  
A: AWS Shield Standard — it's enabled automatically on all accounts at no extra charge.

**Q: Which service tracks configuration changes over time?**  
A: AWS Config (not CloudTrail). CloudTrail tracks API calls. Config tracks resource configuration state.

**Q: What's the minimum support plan to get 24/7 phone support?**  
A: Business. Developer only gets business-hours email.

**Q: What's the best way to reduce costs for a database that's only used 8 hours/day?**  
A: Move to a managed serverless option (like Aurora Serverless) — it scales to zero when not in use.

**Q: What's the difference between an AWS Region and an Edge Location?**  
A: A Region runs compute, storage, and databases. An Edge Location only caches content for CloudFront. You cannot launch EC2 in an Edge Location.

---

## Final Revision Tips

1. **Know the shared responsibility model cold** — it's in ~20% of questions directly or indirectly.
2. **Security services** — be able to tell GuardDuty / Inspector / Macie / Config / CloudTrail apart.
3. **Support plan tiers** — especially what each level adds over the previous.
4. **Pricing model scenarios** — match the workload description to the right EC2 pricing model.
5. **Global infrastructure terms** — Region, AZ, Edge Location, Local Zone, Outposts.
6. **Don't overthink it** — the CLF-C02 tests breadth, not depth. Exam questions are scenario-based but not deeply technical.
