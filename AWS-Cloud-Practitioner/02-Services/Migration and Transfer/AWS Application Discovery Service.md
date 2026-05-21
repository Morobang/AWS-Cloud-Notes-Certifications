# AWS Application Discovery Service

## What you'll learn
- What Application Discovery Service does
- Agent vs agentless discovery
- Integration with Migration Hub
- Key exam facts

---

## What is AWS Application Discovery Service?

AWS Application Discovery Service **collects information about on-premises servers and databases** to help plan a migration to AWS. Before you can migrate, you need to know what you have — which servers exist, what's running on them, how they're connected, and how much capacity they use.

---

## Two discovery modes

**Agentless discovery (Application Discovery Connector)** — A VMware virtual appliance deployed in your VMware vCenter environment. It collects:
- Server hostnames, IP addresses
- CPU and memory allocation
- Disk and network utilization
- VM configurations
- Does not require installing software on each server. Works for VMware environments only.

**Agent-based discovery (Application Discovery Agent)** — A lightweight agent installed on each Windows or Linux server. It collects:
- Server utilization (CPU, memory, disk, network)
- System performance
- **Running processes** and **network connections** between servers
- More detailed than agentless — captures application dependencies
- Works on any server (physical or virtual, any hypervisor)

---

## Dependency mapping

The agent-based approach captures network connections between servers, revealing **application dependencies** — which servers communicate with which, on which ports. This is critical for migration planning: if Server A always connects to Server B, they should be migrated together (as a group) or in the right order.

---

## Integration with Migration Hub

All data collected by Application Discovery Service flows into **AWS Migration Hub**, which provides a central view of discovered servers, their utilization, and migration status.

---

## Application Discovery Service vs Migration Evaluator

| | Application Discovery Service | Migration Evaluator |
|-|------------------------------|---------------------|
| Primary output | Server inventory and dependency map | TCO comparison / business case |
| Focus | Technical: what servers exist, their specs | Financial: what it costs now vs on AWS |
| Used by | Migration engineers, architects | Business decision makers |

---

## Exam focus

- Application Discovery Service = **discover and inventory on-premises servers** before migration
- **Agentless**: VMware environments, no server software needed
- **Agent-based**: Any server; reveals process and network dependency maps
- Data feeds into **Migration Hub** for centralized tracking
- Use when exam describes: "discover what servers exist before migration," "map application dependencies," "inventory on-premises infrastructure"

---

## Practice questions

**Q1.** A company wants to migrate 200 on-premises servers to AWS but needs to understand which servers communicate with each other to plan migration groups. Which AWS service collects this dependency information?

A) Migration Evaluator  
B) AWS Migration Hub  
C) AWS Application Discovery Service  
D) AWS Config

**Answer: C** — Application Discovery Service with the agent-based collector captures network connections between servers, revealing application dependencies. Migration Evaluator builds cost comparisons. Migration Hub provides centralized tracking but relies on discovery data from Application Discovery Service. Config tracks AWS resource configuration, not on-premises servers.
