# AWS Database Migration Service (AWS DMS)

**Category**: Data Migration and Ingestion  
**Exam weight**: High — essential for database-to-AWS migrations and ongoing CDC replication

---

## What Is It?

AWS DMS is a managed service for migrating databases to or within AWS. It supports one-time migrations and ongoing continuous replication using Change Data Capture (CDC).

**DMS replication modes:**
1. **Full load**: Migrates all existing data from source to target
2. **Full load + CDC**: Migrates all existing data, then continues replicating ongoing changes
3. **CDC only**: Replicates ongoing changes only (assumes tables already exist in target)

---

## How DMS Works

```
Source DB (on-premises or AWS)
    ↓
DMS Replication Instance (EC2-based)
  - Connects to source
  - Reads changes
  - Applies transforms (optional)
    ↓
Target DB (AWS or on-premises)
```

**Replication instance**: A managed EC2 instance you provision. Size it based on data volume and change rate. Multi-AZ option for high availability.

---

## Source and Target Support

**Source databases:**
- Oracle, SQL Server, MySQL, PostgreSQL, MariaDB (on-premises or RDS)
- Amazon Aurora, Amazon RDS
- SAP ASE (Sybase)
- IBM Db2
- MongoDB, Amazon DocumentDB
- Amazon S3 (as source for file-based migrations)

**Target databases:**
- Amazon RDS, Aurora, Redshift, DynamoDB, S3, OpenSearch, Kinesis Data Streams, MSK
- On-premises databases (for migration back, or for replication in reverse)

---

## Change Data Capture (CDC)

CDC replicates ongoing database changes in near-real-time.

**How CDC works (PostgreSQL example):**
1. DMS reads the database **transaction log** (WAL in PostgreSQL, binary log in MySQL, redo log in Oracle)
2. Every INSERT/UPDATE/DELETE is captured as a change event
3. DMS applies those changes to the target in the same order

**For PostgreSQL source**: DMS uses logical replication slots. This keeps the WAL file open — if DMS falls behind, WAL files accumulate and disk fills up. Monitor `ReplicationSlotDiskUsage` and `CDCLatencySource`.

**For MySQL source**: DMS uses binary logging. Enable `binlog_format=ROW` on the source MySQL server.

### CDC with S3 as Target

Useful for building a change log in the data lake:

```
Production MySQL
    ↓ DMS CDC
Amazon S3 (CSV files, one per batch)
  ├── load-00000001.csv  (INSERT records)
  ├── load-00000002.csv  (UPDATE records)
  └── load-00000003.csv  (DELETE records)
    ↓ Glue job
Merged/applied Iceberg table in S3
```

Each CDC file contains the operation type (`I`, `U`, `D`) and full row data.

---

## AWS Schema Conversion Tool (AWS SCT)

DMS handles data migration; SCT handles schema migration.

**Use SCT when migrating between different database engines (heterogeneous):**

```
Oracle → Amazon Aurora PostgreSQL
SQL Server → Amazon RDS MySQL
Oracle → Amazon Redshift
```

SCT:
1. Analyzes the source schema
2. Converts supported objects automatically
3. Flags unsupported objects with a manual action required
4. Generates a migration assessment report

**DMS + SCT workflow:**
1. Run SCT → convert schema → fix manual items → create target schema
2. Run DMS Full Load → migrate data
3. Run DMS CDC → replicate ongoing changes while you test
4. Cutover: stop writes to source, let CDC drain, switch application to target

### Homogeneous vs Heterogeneous

| Type | Example | Schema conversion |
|---|---|---|
| Homogeneous | MySQL → Amazon RDS MySQL | Not needed — same engine |
| Heterogeneous | Oracle → Aurora PostgreSQL | Use SCT before DMS |

---

## DMS Endpoints

DMS uses **endpoints** to define source and target connections:

```python
import boto3
dms = boto3.client('databasemigrationservice')

# Create a source endpoint
dms.create_endpoint(
    EndpointIdentifier='source-mysql',
    EndpointType='source',
    EngineName='mysql',
    ServerName='mysql.internal.example.com',
    Port=3306,
    DatabaseName='production_db',
    Username='dms_user',
    Password='...'  # Or use Secrets Manager
)
```

Use Secrets Manager to store database credentials instead of passing them directly.

---

## DMS Tasks

A **replication task** connects a source endpoint to a target endpoint and defines what to migrate:

**Task settings:**
- Migration type: Full load / Full load + CDC / CDC only
- Table mapping rules: include/exclude specific tables or columns
- Transformation rules: rename tables/columns, uppercase/lowercase, add prefix/suffix
- Parallel load settings: how many tables to load simultaneously

**Table mapping example (include only the sales schema):**
```json
{
  "rules": [
    {
      "rule-type": "selection",
      "rule-id": "1",
      "rule-name": "include-sales",
      "object-locator": {
        "schema-name": "sales",
        "table-name": "%"
      },
      "rule-action": "include"
    }
  ]
}
```

---

## Data Validation

DMS can validate that source and target data match after migration:

- Counts rows in source vs target
- Compares row checksums
- Reports mismatches as validation failures

Enable with `EnableValidation=true` in task settings.

---

## DMS Monitoring

| Metric | What it means |
|---|---|
| `CDCLatencySource` | Time lag between DB write and DMS capturing it |
| `CDCLatencyTarget` | Time lag between DMS capturing and writing to target |
| `FreeableMemory` | Available memory on replication instance |
| `ReplicationSlotDiskUsage` | WAL accumulation (PostgreSQL sources) |

High `CDCLatencyTarget` → target is slow (e.g., Redshift can't keep up with inserts). Batch more changes per transaction or scale the target.

---

## Exam Scenarios

| Scenario | Answer |
|---|---|
| Migrate on-premises Oracle database to Amazon Aurora PostgreSQL | SCT (schema conversion) + DMS (data migration) |
| Replicate ongoing changes from production MySQL to a Redshift data warehouse | DMS with CDC mode |
| Migrate MySQL to Amazon RDS MySQL | DMS alone (homogeneous — no SCT needed) |
| Stream database change events to Kinesis Data Streams | DMS with Kinesis as the target endpoint |
| DMS replication is lagging and CDCLatencySource is growing | Scale up replication instance, reduce source load, or optimize target write performance |
