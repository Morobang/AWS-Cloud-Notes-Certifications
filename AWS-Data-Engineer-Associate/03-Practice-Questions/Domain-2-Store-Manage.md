# Domain 2: Store and Manage Data — Practice Questions

> 20 questions | ~26% of the DEA-C01 exam

---

**Q1.** A company needs to store financial transaction data with the following requirements: sub-10ms read latency, 200,000 writes per second, the primary access pattern is lookup by transaction ID, and the schema may evolve over time. Which service is most appropriate?

A) Amazon RDS MySQL  
B) Amazon Redshift  
C) Amazon DynamoDB  
D) Amazon S3 + Athena

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

DynamoDB is designed for high-throughput key-value access at single-digit millisecond latency. It handles 200,000 WPS with on-demand or provisioned capacity, supports flexible schema (each item can have different attributes), and lookup by partition key (transaction ID) is its primary access pattern.

- A (RDS MySQL) cannot scale writes to 200,000 WPS and has fixed schema.
- B (Redshift) is an analytical warehouse — not suited for high-frequency transactional writes.
- D (S3 + Athena) queries take seconds and S3 is not a transactional database.
</details>

---

**Q2.** A data engineer designs a Redshift star schema. The fact table has 5 billion rows and is frequently joined to a customer dimension table that has 50,000 rows. Queries joining these tables are slow. What is the BEST optimization?

A) Change the fact table distribution to EVEN  
B) Change the customer dimension table distribution to ALL  
C) Add a compound sort key on the fact table's customer_id column  
D) Use Redshift Serverless instead of provisioned clusters

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Distributing the dimension table with `ALL` replicates a full copy to every compute node. When the fact table (already spread across nodes) joins the dimension, there's no data movement — each node has the dimension data it needs locally.

- A (EVEN distribution on fact table) distributes rows randomly — JOINs still require network data movement.
- C (sort key on customer_id) helps range scans and filters but doesn't eliminate JOIN data movement.
- D (Serverless) changes the pricing model but doesn't fix the distribution key design issue.
</details>

---

**Q3.** A company stores customer order data in DynamoDB with partition key `customer_id` and sort key `order_date`. They need to add a new access pattern: "find all orders with status = 'PENDING' across all customers." What is the BEST approach?

A) Scan the entire table and filter by status  
B) Create a Global Secondary Index with partition key = status  
C) Create a Local Secondary Index with sort key = status  
D) Duplicate the table with status as the partition key

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

A Global Secondary Index (GSI) with `status` as the partition key enables efficient queries for `status = 'PENDING'` without scanning the entire table.

- A (Scan) reads every item in the table — extremely expensive and slow at scale.
- C (LSI) requires the same partition key as the table — it can only help for `customer_id + status` queries, not status queries across all customers.
- D creates data duplication and sync complexity — GSI is purpose-built for this.
</details>

---

**Q4.** A data lake on S3 needs to support GDPR right-to-erasure requests. The data is stored in Parquet files with billions of rows, partitioned by date. When a user requests erasure, their records must be deleted. What is the BEST technical approach?

A) Use S3 Object Lock to mark objects for deletion  
B) Migrate the data to Apache Iceberg format and use Iceberg row-level deletes  
C) Re-ingest all data to a new bucket excluding the deleted user  
D) Mark the user as deleted in a separate DynamoDB table and filter in queries

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Apache Iceberg supports row-level deletes and MERGE operations on S3 data. After deleting the rows, compaction jobs clean up the Parquet files. This is the scalable, purpose-built solution for GDPR-compliant data lakes.

- A (Object Lock) prevents deletions — the opposite of what GDPR requires.
- C (re-ingesting all data) is impractically expensive and time-consuming at petabyte scale.
- D (soft delete flag) doesn't actually remove the data from S3 — regulators require physical deletion.
</details>

---

**Q5.** A company's data lake stores events in raw JSON on S3. Over time, the producer started adding new fields to the JSON payload. Existing queries in Athena return errors on the new fields. What should the data engineer do?

A) Re-process all existing files to add the new fields  
B) Create a new table in the Glue Data Catalog for the new schema  
C) Enable schema merging in the Glue Data Catalog table  
D) Convert all files to CSV format

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Glue Data Catalog supports schema merging, which creates a superset schema from files with different column sets. Old files return NULL for new fields; new files return the actual values. No data reprocessing required.

- A is unnecessary and expensive — schema evolution should be handled without reprocessing.
- B creates two separate tables — existing queries that reference the original table still fail.
- D (CSV) doesn't support schema evolution and is a worse format for analytics.
</details>

---

**Q6.** A Redshift data warehouse is experiencing slow queries. The DBA runs `EXPLAIN` on a slow query and sees that many rows are being returned from the storage scan before filtering. What maintenance operation is most likely to help?

A) Run VACUUM FULL on the table  
B) Run ANALYZE on the table  
C) Increase the number of nodes  
D) Convert the table to EVEN distribution

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

`ANALYZE` updates the statistics used by the Redshift query planner. Without current statistics, the planner may choose poor execution plans — for example, doing a full scan instead of using zone maps. Run ANALYZE after large data loads.

- A (VACUUM) reclaims space from deleted rows and re-sorts data — helps with sort key ordering, not query planner statistics.
- C (more nodes) increases capacity but doesn't fix a stale statistics problem.
- D (EVEN distribution) may help with JOINs but doesn't fix query planner statistics.
</details>

---

**Q7.** A data engineer needs to store streaming IoT sensor data. Each device sends 10 readings per second. Queries always look up the latest N readings for a specific device. The system must handle 50,000 devices. Which DynamoDB key design is BEST?

A) Partition key: `reading_id` (UUID), no sort key  
B) Partition key: `device_id`, sort key: `timestamp`  
C) Partition key: `sensor_type`, sort key: `device_id`  
D) Partition key: `timestamp`, no sort key

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

`device_id` as the partition key distributes traffic across 50,000 partitions (high cardinality). `timestamp` as the sort key enables range queries: "get the last 100 readings for device X" = `device_id = X ORDER BY timestamp DESC LIMIT 100`.

- A (UUID as partition key) distributes data but makes range queries by device impossible.
- C (sensor_type as partition key) is low cardinality — creates hot partitions with millions of items per partition.
- D (timestamp as partition key) creates a hot partition at the current timestamp — all 50,000 devices writing to the same partition.
</details>

---

**Q8.** A company needs to query structured customer data from RDS, semi-structured event logs from S3, and telemetry from OpenSearch in a single analytical query. What is the BEST approach?

A) Migrate all data to Amazon Redshift  
B) Use Amazon Athena Federated Query with data source connectors  
C) Copy all data to DynamoDB and query from there  
D) Use AWS Glue to join the data sources and write to S3, then query with Athena

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Athena Federated Query allows SQL queries that span multiple data sources (S3, RDS, OpenSearch, DynamoDB, CloudWatch) using Lambda data source connectors. No data movement required.

- A (migrate to Redshift) is a valid option but requires ETL pipelines and duplicates data — Federated Query is simpler for ad-hoc cross-source queries.
- C (DynamoDB) is not an analytical service — SQL JOINs across sources isn't its use case.
- D is correct for building a persistent joined dataset, but for ad-hoc queries Federated Query is more direct.
</details>

---

**Q9.** A data engineer configures S3 lifecycle rules. A regulation requires all financial audit logs to be retained for exactly 7 years, then deleted. They must also be immutable during the retention period. Which S3 features implement this?

A) S3 Versioning + lifecycle expiration at 7 years  
B) S3 Object Lock in Governance mode + lifecycle expiration  
C) S3 Object Lock in Compliance mode + lifecycle expiration  
D) S3 Glacier storage class + S3 Glacier Vault Lock

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

S3 Object Lock in Compliance mode prevents any user — including root — from deleting or overwriting objects before the retention date. Lifecycle expiration then deletes the object after 7 years. Governance mode allows administrators with a special IAM permission to bypass the lock, which doesn't meet "immutable" requirements.

- A (Versioning + lifecycle) — versioning doesn't prevent deletion; it just retains versions.
- B (Governance mode) allows authorized users to override the lock — not fully immutable.
- D (Glacier Vault Lock) is valid but S3 Object Lock + lifecycle on standard S3 is the more common exam answer.
</details>

---

**Q10.** A company is migrating a 10 TB Oracle database to Amazon Aurora PostgreSQL. The migration must happen with minimal downtime. What is the correct approach and tool combination?

A) AWS Snowball for data transfer, then restore to Aurora  
B) AWS SCT for schema conversion + AWS DMS for data migration with CDC  
C) AWS DMS alone for schema and data migration  
D) AWS DataSync to transfer Oracle data files to S3, then import to Aurora

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Oracle → Aurora PostgreSQL is a heterogeneous migration (different engines). AWS SCT converts the Oracle schema (stored procedures, data types, indexes) to PostgreSQL-compatible DDL. AWS DMS then migrates the data using Full Load + CDC mode — CDC keeps the target in sync during testing and allows near-zero-downtime cutover.

- A (Snowball) is for bulk file/object transfer, not database migration with ongoing sync.
- C (DMS alone) handles data migration but not schema conversion for heterogeneous migrations — SCT is required for Oracle → PostgreSQL.
- D (DataSync) transfers files between storage systems, not relational database schemas and data.
</details>

---

**Q11.** An Aurora PostgreSQL cluster is being used for both OLTP transactions and analytical reporting. Reports are causing performance degradation for transactional users. What is the BEST solution?

A) Migrate reports to Redshift  
B) Create an Aurora read replica and direct analytical queries to it  
C) Increase the Aurora instance size  
D) Enable Aurora Serverless for reporting

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Aurora read replicas share the same storage as the writer — they are up-to-date within milliseconds. Directing analytical queries to a dedicated read replica isolates them from the OLTP write workload with no data movement.

- A (migrate to Redshift) is the right long-term answer for complex analytics but requires a data pipeline and data movement.
- C (larger instance) increases cost but doesn't isolate the workloads.
- D (Aurora Serverless) changes the compute model but doesn't isolate analytical load from OLTP.
</details>

---

**Q12.** A data engineer runs `VACUUM DELETE ONLY` on a Redshift table and notices query performance doesn't improve significantly. Which additional operation is needed?

A) Run `ANALYZE` to update query planner statistics  
B) Run `VACUUM SORT ONLY` to re-sort the table  
C) Increase the number of Redshift nodes  
D) Change the sort key on the table

<details>
<summary>Answer & Explanation</summary>

**Answer: A**

`VACUUM DELETE ONLY` reclaims space from deleted rows but does NOT update query planner statistics. `ANALYZE` updates the statistics that the query planner uses to choose execution plans. Both operations are needed after large data modifications.

- B (VACUUM SORT ONLY) re-sorts rows but also doesn't update statistics.
- C (more nodes) doesn't fix the statistics problem.
- D (change sort key) requires recreating the table and is a major operation.
</details>

---

**Q13.** A data lake table in Glue Data Catalog has 5,000 S3 partitions. Athena queries that don't filter by partition run in 30 seconds, but queries that DO filter by partition (e.g., `WHERE year=2024 AND month=1`) are slow because Athena first lists all 5,000 S3 partition paths. What is the BEST fix?

A) Reduce the number of partitions by merging date ranges  
B) Enable Partition Projection in the Athena table definition  
C) Run MSCK REPAIR TABLE to update partition metadata  
D) Convert files from Parquet to CSV

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Partition Projection pre-computes partition ranges in the Athena table definition. Athena generates S3 paths mathematically without listing S3 objects — query start time drops dramatically for tables with many partitions.

- A (merge partitions) loses granularity and is the wrong direction.
- C (MSCK REPAIR TABLE) updates the Glue catalog with current S3 partitions — it doesn't speed up query startup.
- D (CSV) is worse for performance than Parquet.
</details>

---

**Q14.** A data engineer needs to query data from a DynamoDB table in an Athena SQL query alongside S3 data. What is required?

A) Export the DynamoDB table to S3 first  
B) Use Athena Federated Query with a DynamoDB connector Lambda function  
C) Create a Glue table pointing to DynamoDB  
D) Use AWS Glue's DynamoDB source to transform data first

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Athena Federated Query uses Lambda data source connectors to query sources beyond S3. The pre-built DynamoDB connector allows Athena to query DynamoDB in the same SQL statement as S3 data.

- A (export first) works but requires a separate step and results in data that's not real-time.
- C (Glue table pointing to DynamoDB) is not supported — Glue Data Catalog tables reference file-based or JDBC sources.
- D (Glue transform) creates a copy in S3 — works but is a batch approach, not direct query.
</details>

---

**Q15.** A Redshift cluster uses KEY distribution on `customer_id` for the fact table and ALL distribution for dimension tables. Query performance is still poor for aggregations that don't involve JOINs. Which optimization should be applied?

A) Change fact table distribution to EVEN  
B) Add a compound sort key on the most-filtered column  
C) Change dimension tables to KEY distribution  
D) Enable Concurrency Scaling

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Sort keys allow Redshift to skip large ranges of data using zone maps (min/max per 1 MB block). For aggregations that filter by a column (e.g., `WHERE order_date >= '2024-01-01'`), a sort key on `order_date` allows Redshift to skip blocks outside the date range.

- A (EVEN distribution) helps with JOIN data movement but not filter-based aggregation performance.
- C (dimension tables to KEY) is incorrect — small dimension tables should stay ALL.
- D (Concurrency Scaling) adds capacity for concurrent queries but doesn't optimize individual query performance.
</details>

---

**Q16.** A data engineer uses DynamoDB TTL to expire session records after 24 hours. The engineer notices deleted items still appear in DynamoDB Streams for up to 48 hours after the TTL timestamp. Is this expected behavior?

A) No — TTL deletes are immediate  
B) Yes — DynamoDB TTL processes expiration within 48 hours of the timestamp  
C) No — TTL deletes should not appear in Streams  
D) Yes — TTL deletes are queued and processed in batches weekly

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

DynamoDB TTL does not guarantee immediate deletion. Items are typically deleted within 48 hours of the expiration timestamp. TTL deletions do appear in DynamoDB Streams as DELETE events, allowing consumers to react to expirations.

This is expected behavior — design applications to tolerate expired items appearing for up to 48 hours after the TTL timestamp.
</details>

---

**Q17.** A company uses Redshift Spectrum to query S3 data. Analysts report that Spectrum queries are slow and expensive. Which optimization addresses BOTH speed and cost?

A) Convert S3 files from CSV to Parquet with Snappy compression, and partition by query date  
B) Load S3 data into Redshift tables before querying  
C) Increase the number of Redshift compute nodes  
D) Enable Redshift result caching

<details>
<summary>Answer & Explanation</summary>

**Answer: A**

Parquet reduces data scanned (columnar — only needed columns read) and Snappy compression reduces bytes. Partitioning by date ensures only the relevant date range is scanned. Spectrum charges per TB scanned — less data = faster and cheaper.

- B (load into Redshift) increases storage cost and ETL complexity — Spectrum exists specifically to avoid this.
- C (more nodes) doesn't reduce the amount of S3 data scanned.
- D (result caching) only helps for identical repeated queries.
</details>

---

**Q18.** A company needs a data store for a fraud detection graph — modeling relationships between customers, accounts, merchants, and transactions. Queries need to traverse: "find all accounts connected to this merchant within 3 hops." Which service is BEST?

A) Amazon DynamoDB  
B) Amazon Neptune  
C) Amazon Redshift  
D) Amazon OpenSearch

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Neptune is a graph database purpose-built for storing and traversing relationship networks. Graph traversal ("find nodes within N hops") is Neptune's native query pattern, using Gremlin or openCypher.

- A (DynamoDB) requires denormalizing the graph into flat items — graph traversals require many reads and can't be expressed efficiently.
- C (Redshift) can do recursive SQL CTEs for graphs but is orders of magnitude slower than a native graph database.
- D (OpenSearch) is optimized for full-text search, not graph relationship traversal.
</details>

---

**Q19.** An analytics team queries a Glue Data Catalog table backed by S3. Hourly jobs add new partitions, but Athena doesn't see the new data. What is the BEST automated solution?

A) Manually run `MSCK REPAIR TABLE` after each job  
B) Configure the Glue ETL job to call `create_partition` via the Glue API after writing data  
C) Enable Partition Projection to eliminate the need for partition registration  
D) Switch to Apache Iceberg format for automatic partition discovery

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Partition Projection removes the need to register new partitions — Athena computes partition paths from defined ranges (e.g., dates). New hourly partitions are automatically available without any catalog updates.

- A (manual MSCK REPAIR) doesn't scale — requires human action every hour.
- B (call create_partition via API) automates it, but still requires an extra step. Partition Projection eliminates the step entirely.
- D (Iceberg) also provides automatic discovery but requires migrating the table format.
</details>

---

**Q20.** A company needs to retain customer data in S3 for 10 years but make the data available for analytics for the first 2 years and then archived for compliance only. Which S3 configuration implements this MOST cost-effectively?

A) Store all data in S3 Standard for 10 years  
B) Lifecycle rule: Standard for 2 years → Glacier Flexible for 8 years → delete at 10 years  
C) Lifecycle rule: Standard for 2 years → Glacier Deep Archive for 8 years → delete at 10 years  
D) Lifecycle rule: Standard-IA immediately → Glacier after 1 year → delete at 10 years

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Glacier Deep Archive is the cheapest storage class ($0.00099/GB/month) — ideal for compliance archives that are rarely accessed. The data is accessible for the first 2 years (Standard), then moves to Deep Archive for 8 years.

- A (Standard for 10 years) is the most expensive option.
- B (Glacier Flexible) is cheaper than Standard but more expensive than Deep Archive. For data that is "compliance only" (not regularly accessed), Deep Archive is appropriate.
- D (Standard-IA immediately) is incorrect if the data is actively used for analytics in the first 2 years — Standard is better for active access.
</details>
