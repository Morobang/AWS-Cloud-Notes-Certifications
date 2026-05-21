# AWS Lake Formation

**Category**: Data Lake Governance  
**Exam weight**: Medium-High — appears in Domain 2 and Domain 4

---

## What Is It?

AWS Lake Formation is a service that makes it easier to build, secure, and manage data lakes. It provides centralized access control, data discovery, and governance for data stored in S3 and the Glue Data Catalog.

Think of it as the security layer for your data lake — instead of managing complex S3 bucket policies and IAM policies per user, you define fine-grained permissions in one place.

---

## Core Value Proposition

**Without Lake Formation:**
- Manage S3 bucket policies for each bucket
- Manage IAM policies for each user/role
- No column-level or row-level security
- Hard to audit who accessed which data

**With Lake Formation:**
- Single console to manage all data lake permissions
- Column-level security: grant access to specific columns only
- Row-level filtering: users see only rows matching a filter
- Tag-based access control
- Cross-account data sharing

---

## Key Concepts

### Lake Formation Permissions

Lake Formation adds a permissions layer on top of IAM:

1. **IAM permissions** must allow the action (first gate)
2. **Lake Formation permissions** must also allow it (second gate)

Both must be satisfied for a query to succeed.

### Data Lake Locations

Register S3 paths as "data lake locations" — Lake Formation now controls access to those paths in the Glue Data Catalog.

### Principals

Any IAM user, IAM role, SAML user, or Active Directory group can be a principal.

---

## Permission Types

### Table-Level Permissions

```
GRANT SELECT ON DATABASE my_db TABLE orders TO ROLE analyst_role;
GRANT INSERT, DELETE ON DATABASE my_db TABLE orders TO ROLE etl_role;
```

### Column-Level Security

Grant access to specific columns only:

```
GRANT SELECT ON DATABASE my_db TABLE customers 
  COLUMNS (customer_id, name, region)   -- can see these
  TO ROLE analyst_role;
  -- NOT ssn, credit_card, dob
```

### Row-Level Filtering (Data Filters)

Create a filter that limits which rows a principal can see:

```
Filter name: regional_filter
Filter expression: region = 'ZA'
Apply to role: sa_analyst_role
```

South African analysts can only query rows where `region = 'ZA'`.

### Tag-Based Access Control (LF-TBAC)

Assign LF-tags to databases/tables/columns, then grant access to tags instead of specific tables:

```
Tag: sensitivity=public     → accessible to all analysts
Tag: sensitivity=confidential → accessible to senior analysts only
Tag: sensitivity=restricted → accessible to data owners only
```

Scale to thousands of tables without managing individual permissions.

---

## Cross-Account Data Sharing

Producer account: Has the data + Data Catalog
Consumer account: Wants to query the data

1. Producer grants Lake Formation permissions to the Consumer account's IAM role
2. Consumer uses RAM (Resource Access Manager) to accept the share
3. Consumer queries data using Athena/Glue/Redshift Spectrum as if it were local

No data is copied — consumers query the data in the producer's S3 bucket securely.

---

## Integration with AWS Services

| Service | Lake Formation Integration |
|---|---|
| Amazon Athena | Respects LF column/row permissions on all queries |
| AWS Glue ETL | Respects LF permissions when reading/writing catalog tables |
| Amazon Redshift Spectrum | Respects LF permissions for external tables |
| Amazon EMR | Respects LF permissions with the LF connector |
| Amazon QuickSight | Can use LF row-level security in dashboards |

---

## Governed Tables (Lake Formation Tables)

A newer feature — tables managed by Lake Formation with:
- **ACID transactions**: Multiple writers without conflicts
- **Automatic compaction**: Merges small files into larger ones
- **Row-level access control built-in**: LF permissions applied automatically

Good for: mutable data lake tables where multiple pipelines write concurrently.

---

## Audit Trail

Lake Formation logs all data access attempts to CloudTrail:
- Who queried which table
- Which columns were accessed
- Whether access was granted or denied

This is the audit trail regulators want to see for data governance compliance.

---

## Exam Scenarios

| Scenario | Answer |
|---|---|
| Grant column-level access to Athena users | AWS Lake Formation column permissions |
| Different regions' analysts see only their region's data | Lake Formation row-level filters |
| Scale permissions across 1000 tables by data sensitivity | Lake Formation tag-based access control (LF-TBAC) |
| Share data catalog tables cross-account securely | Lake Formation + RAM cross-account sharing |
| Need audit trail of who accessed which S3 data | Lake Formation + CloudTrail data events |
| Multiple ETL jobs writing to the same S3 path concurrently | Lake Formation Governed Tables (ACID) |
