# Migration Evaluator

## What you'll learn
- What Migration Evaluator does
- The business case for cloud migration
- Key exam facts

---

## What is Migration Evaluator?

Migration Evaluator (formerly TSO Logic) is a service that **helps organizations build a data-driven business case for migrating to AWS**. It analyzes your current on-premises environment — hardware, software, and utilization — and produces a cost comparison showing what your current environment costs vs what it would cost on AWS.

---

## What it does

**Inventory collection** — Migration Evaluator can install an agentless collector that reads utilization data from VMware vCenter, SQL Server, or other sources. Alternatively, you can import data manually.

**Right-sizing analysis** — It analyzes actual CPU and memory utilization to recommend appropriately sized AWS instances — avoiding over-provisioning.

**Cost modeling** — It builds a cost model comparing:
- Current on-premises costs (hardware depreciation, maintenance, power, cooling, licensing, staff)
- Projected AWS costs (On-Demand, Reserved Instances, Savings Plans) for right-sized instances

**Business case report** — The final deliverable is a report showing estimated savings (typically 3-year TCO comparison) to support migration decisions.

---

## Migration Evaluator vs Application Discovery Service

| | Migration Evaluator | Application Discovery Service |
|-|--------------------|-----------------------------|
| Primary output | Business case / cost comparison | Server inventory and utilization data |
| Focus | Financial justification | Technical discovery |
| Depth | Cost modeling, TCO analysis | CPU, memory, network, storage per server |
| Used by | Business decision makers, finance | Migration engineers, architects |

Both collect server data, but they serve different purposes.

---

## When to use it

**Migration business case** — Finance or leadership needs a quantified cost comparison to approve cloud migration spending.

**Right-sizing planning** — Understand actual utilization before migration to avoid over-provisioning EC2 instances.

---

## Exam focus

- Migration Evaluator = **business case and TCO analysis** for migrating to AWS
- Collects utilization data and produces a cost comparison (on-premises vs AWS)
- Used in the **assess phase** of migration
- Use when exam describes: "build a business case for migration," "compare on-premises costs to AWS costs," "TCO analysis"

---

## Practice questions

**Q1.** A company's leadership needs a financial justification showing total cost of ownership comparison before approving an AWS migration project. Which AWS service produces this type of business case analysis?

A) AWS Application Discovery Service  
B) Migration Evaluator  
C) AWS Migration Hub  
D) AWS Cost Explorer

**Answer: B** — Migration Evaluator collects on-premises utilization data and produces a cost comparison showing current on-premises costs vs projected AWS costs — designed specifically to support migration business cases. Application Discovery Service inventories servers for technical planning but doesn't produce financial TCO comparisons. Migration Hub tracks migration progress. Cost Explorer analyzes existing AWS spending.
