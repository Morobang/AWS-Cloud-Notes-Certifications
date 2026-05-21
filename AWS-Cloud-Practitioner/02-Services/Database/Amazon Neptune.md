# Amazon Neptune

## What you'll learn
- What a graph database is and when you need one
- What Neptune is and what query languages it supports
- Key Neptune use cases
- Exam facts

---

## What is Amazon Neptune?

Amazon Neptune is a **fully managed graph database service**. It is designed for data where the **relationships between items are as important as the items themselves** — for example, social networks, recommendation engines, fraud detection, and knowledge graphs.

---

## What is a graph database?

A graph database stores data as **nodes** (entities) and **edges** (relationships between entities), with **properties** on both.

**Example — social network**:
- Nodes: Alice, Bob, Carol, AWS (a company)
- Edges: Alice *follows* Bob, Bob *works at* AWS, Carol *follows* Alice
- Query: "Find all people Alice follows who work at AWS"

A relational database can answer this with JOINs, but for deeply connected data (friends of friends of friends), the JOIN chains become exponentially slow. Graph databases are designed to traverse these connections efficiently.

---

## Neptune supports two query languages

**Gremlin** — A graph traversal language used for property graph queries. Part of the Apache TinkerPop framework.

**SPARQL** — A query language for RDF (Resource Description Framework) graphs, used for knowledge graphs and semantic data.

Neptune also supports the **openCypher** query language, which is popular with Neo4j users.

---

## When to use Neptune

**Social networks** — Find mutual friends, recommend new connections, identify influencers.

**Fraud detection** — Identify suspicious relationship patterns in financial transactions or account behavior (rings of connected entities).

**Knowledge graphs** — Build a graph of interconnected concepts, entities, and their relationships for search or AI applications.

**Recommendation engines** — "Customers who bought X also bought Y" — model item-to-item relationships and customer-to-item interactions.

**Identity graphs** — Connect customer identities across devices, channels, and platforms.

**Network and IT operations** — Model infrastructure dependencies (which services depend on which servers, which network segments are connected).

---

## Neptune vs other databases

| | Neptune | RDS/Aurora | DynamoDB |
|-|---------|------------|---------|
| Data model | Graph (nodes + edges) | Relational (tables) | Key-value / Document |
| Strength | Traversing relationships | Complex SQL queries | High-scale key lookups |
| Use case | Connected data, social networks | Traditional applications | NoSQL workloads |

---

## Exam focus

- Neptune = **graph database** — for highly connected data where relationships matter
- Managed service — AWS handles infrastructure, patching, backups
- **Highly available**: 6 copies of data across 3 AZs (similar to Aurora)
- Use when exam describes: "social network," "recommendation engine," "fraud detection through relationships," "knowledge graph," "connections between entities"
- Key differentiator: Neptune is for **relationship-heavy** data; RDS is for structured tabular data; DynamoDB is for key-value/document data

---

## Practice questions

**Q1.** A fintech company wants to detect fraud by analyzing patterns of financial transactions to identify connected rings of suspicious accounts. They need to query complex multi-hop relationship chains between entities efficiently. Which database service is most appropriate?

A) Amazon RDS  
B) Amazon DynamoDB  
C) Amazon Neptune  
D) Amazon Redshift

**Answer: C** — Neptune is a graph database designed for exactly this: traversing complex multi-hop relationship chains between entities. Fraud rings involve accounts connected through transactions, shared attributes, and other relationships — a graph query can find these patterns in milliseconds. RDS would require complex and slow JOIN chains. DynamoDB has no native graph traversal. Redshift is for analytical batch queries.
