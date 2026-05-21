# Migration and Transfer — Category Overview

AWS Migration and Transfer services help organizations move applications, databases, and data from on-premises environments (or other clouds) to AWS. The migration journey typically follows a discover → plan → migrate sequence.

---

## Services in this category

| Service | Phase | Purpose |
|---------|-------|---------|
| Migration Evaluator | Assess | Build a business case for cloud migration |
| AWS Application Discovery Service | Assess | Discover and inventory on-premises servers |
| AWS Migration Hub | Plan + Track | Central dashboard to track migration progress |
| AWS Application Migration Service | Migrate | Lift-and-shift servers to AWS (rehost) |
| AWS Database Migration Service (DMS) | Migrate | Migrate databases to AWS |
| AWS Schema Conversion Tool (SCT) | Migrate | Convert database schema to a different engine |
| AWS Snow Family | Migrate | Physical data transfer for large datasets |

---

## The migration phases

**Assess**: Understand what you have and build a business case (Migration Evaluator, Application Discovery Service).

**Mobilize**: Plan the migration in detail, fix gaps (Migration Hub for tracking).

**Migrate and modernize**: Move servers (Application Migration Service), databases (DMS + SCT), and large data volumes (Snow Family).

---

## Exam selection guide

- "Discover on-premises servers before migration" → **Application Discovery Service**
- "Build a business case for migrating to AWS" → **Migration Evaluator**
- "Track all migrations from one dashboard" → **Migration Hub**
- "Lift-and-shift servers to EC2" → **Application Migration Service**
- "Migrate a database to RDS" → **AWS DMS**
- "Convert Oracle schema to PostgreSQL" → **AWS SCT**
- "Transfer petabytes of data offline" → **AWS Snow Family**
