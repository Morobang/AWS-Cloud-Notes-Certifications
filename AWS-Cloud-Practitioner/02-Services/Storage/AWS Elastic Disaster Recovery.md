# AWS Elastic Disaster Recovery

## What you'll learn
- What Elastic Disaster Recovery (DRS) is
- How it differs from backup
- RTO and RPO concepts
- Key exam facts

---

## What is AWS Elastic Disaster Recovery?

AWS Elastic Disaster Recovery (DRS) is a **managed disaster recovery service that enables rapid recovery of on-premises and cloud-based applications onto AWS**. It continuously replicates your servers so that if a disaster occurs, you can launch recovery instances on AWS within minutes.

DRS was formerly known as CloudEndure Disaster Recovery.

---

## Backup vs Disaster Recovery

These terms are related but different:

**Backup** — A copy of data at a point in time. Restoring from backup takes time — you restore data, reinstall OS, reconfigure the application. Recovery Time Objective (RTO) is hours to days.

**Disaster Recovery** — The ability to quickly resume operations after a failure. DRS continuously replicates servers (not just data) so you can launch a fully functioning replica in minutes.

| | AWS Backup | AWS Elastic DRS |
|-|-----------|----------------|
| What it protects | Data | Entire servers (OS + apps + data) |
| Recovery time | Hours to days | Minutes |
| Continuous replication | No | Yes |
| Use case | Data loss protection | Full server failover |

---

## Key concepts

**Recovery Time Objective (RTO)** — How quickly you need to recover after a disaster. DRS targets RTO of minutes.

**Recovery Point Objective (RPO)** — How much data loss is acceptable (measured in time). DRS achieves RPO of seconds because replication is continuous.

---

## How DRS works

1. Install the DRS agent on your source servers (on-premises or EC2)
2. DRS continuously replicates the server's data to a staging area in AWS (using low-cost EC2 and EBS)
3. In a disaster, you initiate failover — DRS launches full-size recovery instances from the latest replication state
4. The recovery instances are running on AWS within minutes
5. When the primary site is restored, you can fail back

---

## When to use it

**On-premises to AWS DR** — Primary data center is your on-premises site; AWS is the DR target. If the data center fails, fail over to AWS.

**Cross-Region DR** — Primary site is one AWS Region; DR target is another Region.

**Compliance DR requirements** — Industries that mandate specific RTO/RPO SLAs.

---

## DRS vs Application Migration Service

Both DRS and Application Migration Service (MGN) use similar replication technology:

| | Elastic DRS | Application Migration Service |
|-|------------|------------------------------|
| Purpose | Disaster recovery — run on DR target only during disaster | Migration — permanently move to AWS |
| Typical state | Staging (low-cost) — scaled up only during a disaster | Scale to full production size permanently |
| Failback | Yes — return to primary after recovery | No — migration is one-way |

---

## Exam focus

- Elastic DRS = **disaster recovery** — continuous replication for rapid failover (minutes)
- **RTO of minutes, RPO of seconds**
- Replicates entire servers (OS + applications + data)
- Used for on-premises → AWS DR or cross-Region DR
- Key differentiator from Backup: DRS = full server DR (minutes); Backup = data recovery (hours)

---

## Practice questions

**Q1.** A company needs a disaster recovery solution for their on-premises servers. If their data center fails, they must be able to recover all servers on AWS within 15 minutes, with a data loss of no more than 30 seconds. Which AWS service meets these requirements?

A) AWS Backup  
B) AWS Application Migration Service  
C) AWS Elastic Disaster Recovery  
D) Amazon S3 Cross-Region Replication

**Answer: C** — Elastic DRS provides continuous block-level replication with RPO of seconds and RTO of minutes — meeting both the 15-minute recovery time and 30-second data loss requirements. Backup restores data but takes hours for full server recovery. Application Migration Service is for permanent migration, not DR failover. S3 CRR replicates objects, not server states.
