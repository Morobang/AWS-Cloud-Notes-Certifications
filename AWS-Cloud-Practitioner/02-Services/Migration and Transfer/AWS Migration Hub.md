# AWS Migration Hub

## What you'll learn
- What Migration Hub is and why it's needed
- What it tracks and integrates with
- Key exam facts

---

## What is AWS Migration Hub?

AWS Migration Hub provides a **single location to track the progress of application migrations to AWS** across multiple AWS and partner migration tools. It gives a centralized view of all migration activities so teams can see the status of every server and application being migrated.

---

## The problem it solves

A large migration involves many moving parts:
- Some servers use Application Migration Service for lift-and-shift
- Some databases use DMS
- Some teams use third-party migration tools

Without Migration Hub, you'd need to check multiple places to understand overall migration progress. Migration Hub aggregates all of this into one dashboard.

---

## What Migration Hub provides

**Application grouping** — Organize servers into logical application groups (e.g., group the web server, app server, and database that make up one application). Migrate and track them as a unit.

**Migration tracking** — See the status of each server and application:
- Not Started
- In Progress
- Completed
- Failed

**Integration with AWS migration tools**:
- Application Discovery Service sends server inventory to Migration Hub
- Application Migration Service sends replication and cutover status
- Database Migration Service sends database migration status
- Partner tools (CloudEndure, Carbonite, etc.) also integrate

**Orchestration** — Migration Hub Orchestrator automates and coordinates multi-step migration workflows across different services.

---

## Migration Hub vs Application Discovery Service

| | Migration Hub | Application Discovery Service |
|-|--------------|------------------------------|
| Role | Central tracking dashboard | Server discovery and inventory |
| Phase | Throughout migration | Assess/plan phase |
| Data | Aggregates from multiple tools | Collects from on-premises |

Application Discovery Service feeds data *into* Migration Hub.

---

## Exam focus

- Migration Hub = **central dashboard for tracking migrations** across AWS and partner tools
- Aggregates data from Application Migration Service, DMS, Application Discovery Service
- **Application grouping** — track servers as part of a logical application
- Use when exam describes: "track migration progress," "central migration dashboard," "see status of all migrations in one place"

---

## Practice questions

**Q1.** An organization is migrating 300 servers to AWS using a combination of AWS Application Migration Service and AWS DMS. The project manager needs a single view showing the status of all migrations. Which AWS service provides this centralized tracking?

A) AWS Application Discovery Service  
B) AWS Migration Hub  
C) AWS CloudTrail  
D) AWS Systems Manager

**Answer: B** — Migration Hub provides a single dashboard showing migration status across multiple tools including Application Migration Service and DMS. Application Discovery Service inventories servers but doesn't track active migrations. CloudTrail records API calls. Systems Manager manages operational tasks on EC2 instances.
