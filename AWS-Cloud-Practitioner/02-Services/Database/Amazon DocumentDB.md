# Amazon DocumentDB

## What you'll learn
- What a document database is
- What Amazon DocumentDB is and what it's compatible with
- When to use DocumentDB
- Key exam facts

---

## What is a document database?

A **document database** stores data as semi-structured documents — typically JSON or BSON. Unlike a relational database with fixed rows and columns, each document can have a different structure. This makes document databases well-suited for data that doesn't fit neatly into a rigid schema.

---

## What is Amazon DocumentDB?

Amazon DocumentDB is a **fully managed document database** that is **compatible with MongoDB**. Applications written to work with MongoDB can work with DocumentDB with minimal code changes.

DocumentDB uses a distributed, fault-tolerant storage architecture similar to Aurora — automatically replicating data 6 times across 3 Availability Zones.

---

## MongoDB compatibility

MongoDB is one of the most popular open-source document databases. DocumentDB implements a subset of the MongoDB API, allowing teams to:
- Use existing MongoDB drivers and tools
- Migrate MongoDB workloads to a managed AWS service
- Avoid managing MongoDB infrastructure themselves

---

## Key features

- **Managed service**: AWS handles provisioning, patching, backups, and failover
- **Scalable storage**: Automatically grows in 10 GB increments up to 64 TB
- **High availability**: 6 copies across 3 AZs, automatic failover
- **Up to 15 Read Replicas**: For read scaling
- **Backup and restore**: Continuous backups with point-in-time recovery
- **Security**: Encryption at rest and in transit, VPC isolation

---

## When to use it

**MongoDB migration** — You have an existing MongoDB application and want to run it on a fully managed AWS service without maintaining MongoDB servers.

**Flexible JSON data** — Product catalogs where different products have different attributes; user profile data where fields vary per user.

**Content management** — Storing articles, blog posts, or documents with variable structure.

---

## DocumentDB vs DynamoDB

| | Amazon DocumentDB | Amazon DynamoDB |
|-|------------------|----------------|
| Data model | Document (JSON, MongoDB API) | Key-value and Document |
| Query language | MongoDB query language (MQL) | DynamoDB API / PartiQL |
| Compatibility | MongoDB applications | AWS-native only |
| Scaling | Read Replicas | Serverless auto-scaling |
| Best for | MongoDB migration, complex document queries | High-scale serverless NoSQL |

---

## Exam focus

- DocumentDB = **MongoDB-compatible managed document database**
- Stores **JSON documents** with flexible schema
- Use when migrating MongoDB workloads to AWS
- High availability: **6 copies across 3 AZs** (similar to Aurora architecture)
- Key differentiator from DynamoDB: DocumentDB supports the **MongoDB API**; DynamoDB uses the AWS DynamoDB API
- Use when exam describes: "MongoDB," "document database," "JSON document storage," "MongoDB migration"

---

## Practice questions

**Q1.** A company runs a content management system using MongoDB. They want to migrate to AWS with minimal application code changes and without managing database servers. Which service is most appropriate?

A) Amazon RDS  
B) Amazon DynamoDB  
C) Amazon DocumentDB  
D) Amazon Neptune

**Answer: C** — DocumentDB is MongoDB-compatible, allowing the existing MongoDB application to work with minimal changes. AWS manages the infrastructure. RDS is for relational databases (MySQL, PostgreSQL, etc.). DynamoDB has its own API — not MongoDB-compatible. Neptune is a graph database.
