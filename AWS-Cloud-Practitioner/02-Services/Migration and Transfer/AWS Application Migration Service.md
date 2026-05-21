# AWS Application Migration Service

## What you'll learn
- What Application Migration Service (MGN) does
- How lift-and-shift migration works
- MGN vs the 6 Rs of migration
- Key exam facts

---

## What is AWS Application Migration Service?

AWS Application Migration Service (MGN) is a **managed service that automates the lift-and-shift (rehost) migration of on-premises servers to AWS**. It replicates your physical or virtual servers to AWS EC2 instances, allowing you to run your applications on AWS with minimal changes.

MGN was previously called CloudEndure Migration (after AWS acquired CloudEndure).

---

## How it works

1. **Install agent**: Install the MGN agent on your source server (Windows or Linux, physical or virtual)
2. **Continuous replication**: The agent replicates the server's disks to AWS in near real-time (like a live backup to AWS) — this happens while the original server is still running
3. **Launch test instance**: Launch a test EC2 instance from the replicated data to verify the application works on AWS
4. **Cutover**: At a planned time, perform the actual cutover — launch the production EC2 instance from the latest replicated data, update DNS/routing, and decommission the source server
5. **Minimal downtime**: Because replication is continuous, the cutover window is very short (minutes)

---

## The lift-and-shift approach

Lift-and-shift (rehost) means moving a server to AWS without modifying the application. The server runs on EC2 exactly as it did on-premises — same OS, same application, same configuration.

This is the fastest migration approach (no code changes, no re-architecture) but doesn't take full advantage of cloud-native features. It's often the first step in a phased migration.

---

## The 6 Rs of migration

| Strategy | Description |
|----------|-------------|
| Rehost (Lift-and-shift) | Move to AWS as-is — MGN handles this |
| Replatform | Migrate with minor optimizations (e.g., move to RDS instead of self-managed MySQL) |
| Repurchase | Move to a SaaS product instead |
| Refactor/Re-architect | Redesign to be cloud-native |
| Retire | Decommission — application no longer needed |
| Retain | Keep on-premises for now |

---

## When to use it

**Large-scale rehost migration** — Move hundreds of servers to AWS quickly with minimal downtime and no application code changes.

**Disaster recovery** — Use continuous replication as a DR solution — if on-premises fails, cut over to AWS quickly.

---

## Exam focus

- Application Migration Service = **lift-and-shift migration** — replicates servers to AWS EC2
- Installs an agent that **continuously replicates** server disks to AWS
- Minimal downtime — cutover after replication is caught up
- Formerly called **CloudEndure Migration**
- Use when exam describes: "rehost servers," "lift-and-shift migration," "migrate servers to EC2 with minimal changes"

---

## Practice questions

**Q1.** A company wants to migrate 50 on-premises servers to AWS as quickly as possible without modifying their applications. They need minimal downtime during the cutover. Which AWS service handles this lift-and-shift migration?

A) AWS Database Migration Service  
B) AWS Application Migration Service  
C) AWS Elastic Beanstalk  
D) AWS CloudFormation

**Answer: B** — Application Migration Service replicates on-premises servers to EC2 with continuous replication and minimal cutover downtime — the standard lift-and-shift migration approach. DMS migrates databases specifically. Elastic Beanstalk deploys new applications to managed infrastructure (not migration). CloudFormation provisions infrastructure from templates but doesn't migrate existing servers.
