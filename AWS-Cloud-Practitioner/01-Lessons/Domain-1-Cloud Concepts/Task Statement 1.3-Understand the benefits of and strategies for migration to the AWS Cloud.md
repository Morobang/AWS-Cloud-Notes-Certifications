# Task Statement 1.3: Understand the benefits of and strategies for migration to the AWS Cloud

## What you'll learn
- Why companies migrate from on-premises to the cloud
- The AWS Cloud Adoption Framework (CAF) and its 6 perspectives
- The 6 migration strategies (the "6 R's") and when to use each
- The business benefits of successful migration

---

## Why companies migrate

Most established companies don't start in the cloud — they start with servers in their own buildings (on-premises IT). Over time, problems accumulate:

- Servers age and become expensive to maintain
- Business needs change faster than IT can adapt
- A disaster (fire, flood, power outage) at the building could take down everything
- Scaling up requires months of procurement, shipping, and setup
- IT staff spend most of their time on maintenance instead of building new things

Cloud migration moves IT systems to AWS to gain the benefits covered in Task 1.1: lower costs, faster scaling, global reach, and higher availability. But migration is complicated. Companies have dozens or hundreds of applications with complex interdependencies. You can't just copy everything to the cloud and hope it works — you need a plan.

---

## The AWS Cloud Adoption Framework (CAF)

The **AWS Cloud Adoption Framework** is a structured guide for planning cloud migration. It organizes migration work into **6 perspectives** — 3 focused on business and people, and 3 focused on technical execution.

The reason a framework matters: migrations fail most often not because of technical problems, but because people, governance, and business alignment weren't addressed. CAF makes sure you think through all of these.

### Business perspectives

**Business** — Ensures the migration aligns with actual business goals. Key questions: Why are we migrating? What ROI do we expect? How do we measure success? What business outcomes does cloud enable?

*Example:* A retail chain migrating to enable online sales needs to define success before starting — online revenue targets, time-to-launch, IT cost reductions. Without this, IT delivers a technically successful migration that doesn't move the business forward.

**People** — Addresses the human side of change. Employees need training on new tools. Job roles change when physical servers no longer need manual maintenance. Resistance to change is real and must be managed.

*Example:* IT staff who managed physical servers will need AWS training and certifications. Developers need to learn cloud-native patterns. A change management program helps the organization adapt rather than fight the transition.

**Governance** — Controls and policies for cloud usage. Who approves new cloud resources? How do you prevent runaway spending? How do you ensure compliance with regulations?

*Example:* A healthcare organization needs policies for who can access patient data in the cloud, department-level budget controls, and automated compliance monitoring for HIPAA requirements.

### Technical perspectives

**Platform** — The technical architecture and infrastructure design. Which AWS services to use? How should applications be structured? How do you integrate with existing on-premises systems?

*Example:* Deciding whether to run workloads on EC2 instances, containers, or serverless Lambda functions — and how they connect to existing databases or third-party services.

**Security** — Protecting data and systems in the cloud. Encryption strategy, identity and access management, threat detection, compliance requirements.

*Example:* Designing IAM roles with least privilege, enabling CloudTrail for audit logging, encrypting all data at rest and in transit, setting up GuardDuty for threat detection.

**Operations** — Day-to-day running of cloud systems after migration. Monitoring, incident response, deployment pipelines, capacity management, cost governance.

*Example:* Setting up CloudWatch dashboards and alarms, defining on-call escalation procedures, automating deployments through CI/CD pipelines.

### CAF in one sentence

Business answers *why* to migrate. People answers *who* will be affected and trained. Governance answers *how* to control it. Platform answers *what* to build it on. Security answers *how to protect* it. Operations answers *how to run* it.

---

## The 6 R's of migration

Not every application should be migrated the same way. The **6 R's** give you a framework for choosing the right approach for each application in your portfolio.

---

### Rehost — "Lift and Shift"

Move the application to AWS exactly as it is, with no changes.

**When to use:** Fast migration is the priority; the application works fine as-is; you have limited time or budget for changes; you want quick wins before optimizing.

**Example:** A payroll application running on a company server. Move it to an EC2 instance. Nothing changes functionally — it's just running in AWS instead of on-premises.

**Trade-off:** Fastest and lowest risk, but you don't leverage cloud-native features. Costs may be similar to on-premises at first. The benefit is removing hardware ownership and gaining AWS's uptime and infrastructure reliability.

---

### Replatform — "Lift, Tinker, and Shift"

Move the application with minor modifications to benefit from cloud services, without redesigning the core application.

**When to use:** You want some cloud benefits without a full rebuild; the application could benefit from managed services with minimal code changes.

**Example:** Move a self-managed MySQL database to Amazon RDS (AWS's managed MySQL). The application barely changes, but now AWS handles automated backups, software patching, failover, and scaling automatically. You eliminate an entire category of maintenance work.

**Trade-off:** Moderate effort, meaningful benefits. The practical middle ground for most applications.

---

### Repurchase — "Drop and Shop"

Replace the existing application with a cloud-based SaaS product.

**When to use:** A better cloud-native alternative exists; the existing software is outdated or expensive to maintain; you don't want to manage infrastructure for this function.

**Example:** Replace an on-premises email server (Exchange) with Microsoft 365 or Google Workspace. Instead of managing servers, applying patches, and handling backups for email infrastructure, you pay a monthly subscription and Microsoft or Google manages everything.

**Trade-off:** You get a modern, well-maintained product with professional support. You give up some customization and may need to retrain users on the new system.

---

### Refactor / Re-architect

Redesign the application from the ground up to be cloud-native and take full advantage of cloud capabilities.

**When to use:** The application is strategically critical to the business; it needs significant improvements in performance, scale, or developer experience; the long-term investment is justified.

**Example:** An e-commerce platform built as one large application (monolith) is broken into independent microservices running in containers on ECS, with auto-scaling, serverless components for event-driven work, and managed databases per service.

**Trade-off:** Highest effort and upfront cost, but also the highest long-term benefit. Reserve this for the applications where the investment genuinely pays off.

---

### Retire

Turn off applications that are no longer needed.

**When to use:** The functionality has been replaced by another system; usage is near zero; maintenance costs exceed any value provided.

**Example:** An old reporting system that was replaced by a newer BI platform two years ago, but was never officially decommissioned. Turn it off.

**Trade-off:** Immediate cost savings and reduced complexity, with no downside if the application is truly unused. Most large organizations find 15–30% of their application portfolio can be retired — this is often the biggest quick win in a migration project.

---

### Retain

Leave the application on-premises for now.

**When to use:** The application has complex compliance or hardware dependencies; it's scheduled for replacement soon anyway; migration risk is too high relative to the benefit.

**Example:** A manufacturing system tightly coupled to specialized hardware on the factory floor. Migrate the business applications; keep this one on-premises until it comes up for replacement.

**Trade-off:** No cloud benefits for this application yet, but avoids a high-risk migration. The pragmatic choice when the timing isn't right.

---

## Choosing the right strategy

| Scenario | Best strategy |
|----------|--------------|
| Need to migrate quickly, no time to redesign | Rehost |
| Application works but could benefit from managed services | Replatform |
| Outdated software with a good SaaS alternative | Repurchase |
| Strategically critical application needing modernization | Refactor |
| Application barely anyone uses anymore | Retire |
| Complex hardware dependencies or compliance requirements | Retain |

A real migration project uses multiple strategies across a portfolio. A company might rehost 40%, replatform 20%, repurchase 15%, retire 15%, and refactor only the 10% that are strategically critical.

---

## Migration benefits

**Reduced risk** — Instead of one data center that could be destroyed by fire or flood, your data is replicated across AWS regions and availability zones. AWS provides 99.99%+ uptime SLAs. Recovery time from failures drops from hours to minutes.

**Operational efficiency** — IT staff stop spending time on server maintenance and spend it on building products. Provisioning a new environment takes minutes instead of weeks. Automation eliminates repetitive manual work and human error.

**Increased agility** — Faster infrastructure deployment means faster response to market changes. Launch a new product feature in an afternoon, not after a 6-week hardware procurement cycle.

**Cost alignment** — Instead of fixed infrastructure costs regardless of business performance, cloud costs scale with actual usage. This is especially valuable for businesses with seasonal demand or variable growth.

**ESG improvements** — AWS data centers achieve far better energy efficiency than typical corporate data centers, and AWS is committed to 100% renewable energy. Migration can reduce a company's IT carbon footprint by up to 88%.

---

## Practice questions

**Q1.** A company has a 10-year-old CRM system that works well but requires significant maintenance. They want to reduce IT overhead without major new features. Which migration strategy is most appropriate?

A) Rehost  
B) Repurchase  
C) Refactor  
D) Retain

**Answer: B** — Repurchase replaces the aging system with a maintained SaaS CRM (like Salesforce), eliminating maintenance overhead entirely.

---

**Q2.** A migration planning team is identifying what new skills employees need and designing training programs for the transition. Which AWS CAF perspective does this represent?

A) Business  
B) People  
C) Governance  
D) Operations

**Answer: B** — The People perspective addresses employee skills, role changes, and organizational change management.

---

**Q3.** A company discovers 20 applications in their portfolio that no one has used in over a year. What is the recommended migration strategy?

A) Rehost all of them to keep them available  
B) Refactor them for cloud-native architecture  
C) Retire them  
D) Retain them on-premises indefinitely

**Answer: C** — Retiring unused applications immediately saves licensing, maintenance, and infrastructure costs.

---

**Q4.** A startup needs to migrate quickly to the cloud to reduce hardware costs. They have limited time for changes. Which strategy should they start with?

A) Refactor everything for maximum cloud benefits  
B) Repurchase all applications with SaaS alternatives  
C) Rehost first, then optimize later  
D) Retain everything until a full redesign is ready

**Answer: C** — Rehost is the fastest migration path. Optimize and re-architect once you're in the cloud and understand your actual usage patterns.

---

**Next:** [Task 1.4 — Cloud Economics](./Task%20Statement%201.4-Understand%20concepts%20of%20cloud%20economics.md)
