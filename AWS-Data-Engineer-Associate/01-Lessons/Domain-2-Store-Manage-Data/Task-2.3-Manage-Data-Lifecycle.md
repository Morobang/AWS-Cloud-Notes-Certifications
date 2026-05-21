# Task 2.3: Manage Data Lifecycle

## Learning Objectives
- Design S3 lifecycle policies to automatically transition or expire data
- Understand data retention requirements and how to enforce them
- Know when to use S3 Glacier tiers for archival
- Apply versioning and replication strategies

---

## S3 Lifecycle Policies

Lifecycle rules automatically move or delete objects based on age or other criteria.

### Transition Actions
Move objects to cheaper storage classes as they age:

```
Day 0:   S3 Standard           (frequently accessed, most expensive)
Day 30:  → S3 Standard-IA      (infrequently accessed, cheaper)
Day 90:  → S3 Glacier Instant  (archive, millisecond retrieval)
Day 180: → S3 Glacier Flexible (archive, hours retrieval, cheapest active)
Day 365: → S3 Glacier Deep Archive (cheapest — rarely accessed)
```

### Expiration Actions
Automatically delete objects after a defined number of days.

Example: delete access logs after 90 days.

### Example Lifecycle Rule (JSON)
```json
{
  "Rules": [
    {
      "Filter": {"Prefix": "logs/"},
      "Status": "Enabled",
      "Transitions": [
        {"Days": 30, "StorageClass": "STANDARD_IA"},
        {"Days": 90, "StorageClass": "GLACIER"}
      ],
      "Expiration": {"Days": 365}
    }
  ]
}
```

---

## S3 Versioning

When versioning is enabled, every modification creates a new version — the old version is retained.

**Use cases:**
- Protect against accidental overwrites
- Audit trail of changes to data files
- Required for S3 Cross-Region Replication (CRR)

**Cost consideration**: All versions count toward storage costs. Use lifecycle rules to expire old versions.

**Delete behavior with versioning:**
- "Deleting" an object adds a delete marker — the object still exists
- To permanently delete, you must delete all versions

**S3 Object Lock** (WORM — Write Once Read Many):
- Prevents objects from being deleted or overwritten for a defined period
- Required for certain compliance regulations (SEC Rule 17a-4, HIPAA)
- Works with versioning

---

## S3 Replication

| Type | What it does |
|---|---|
| **Cross-Region Replication (CRR)** | Replicate objects to a bucket in a different Region |
| **Same-Region Replication (SRR)** | Replicate within the same Region (different bucket) |

**Use cases for CRR:**
- Disaster recovery (data in two Regions)
- Compliance (data must be stored in specific geographies)
- Reduce latency for users in different regions

**Requirements:** Source bucket must have versioning enabled.

**Replication is asynchronous** — there's a small lag between write and replication.

---

## Data Retention Strategies

### Retention Types

| Strategy | Implementation |
|---|---|
| Delete after X days | S3 Lifecycle expiration rule |
| Keep for compliance, never modify | S3 Object Lock (WORM) |
| Keep multiple versions, expire old ones | S3 Versioning + lifecycle rules for non-current versions |
| Tiered retention (hot/warm/cold) | S3 lifecycle transitions through storage classes |

### Compliance-Driven Retention

Many industries have legal retention requirements:
- Healthcare (HIPAA): patient data for 6+ years
- Financial (SEC): trading records for 7 years
- GDPR: right to erasure (must be able to delete specific records)

For GDPR compliance: Use field-level encryption or tokenization so deleting the key "erases" the data without physically deleting it.

---

## DynamoDB TTL (Time To Live)

For DynamoDB, you can set a TTL attribute on items. DynamoDB automatically deletes expired items without consuming write capacity.

**How it works:**
1. Store a Unix timestamp in an attribute (e.g., `expires_at`)
2. Enable TTL on that attribute
3. DynamoDB deletes items within 48 hours of the timestamp passing

**Use case:** Session tokens, temporary data, event logs with short retention windows.

```python
import time
import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('sessions')

# Set item to expire in 24 hours
table.put_item(Item={
    'session_id': 'abc123',
    'user_id': 'user001',
    'expires_at': int(time.time()) + 86400  # 24 hours from now (Unix timestamp)
})
```

---

## S3 Storage Class Summary for Data Engineers

| Class | Retrieval | Min storage duration | Use in data lake |
|---|---|---|---|
| Standard | Instant | None | Bronze layer (active ingestion) |
| Intelligent-Tiering | Instant | None | Unknown access patterns |
| Standard-IA | Instant (higher cost) | 30 days | Silver layer (past month) |
| One Zone-IA | Instant | 30 days | Reproducible data (can regenerate) |
| Glacier Instant | Instant | 90 days | Archive with occasional access |
| Glacier Flexible | Minutes–hours | 90 days | Compliance archive |
| Glacier Deep Archive | 12–48 hours | 180 days | Long-term legal archive |

**For data engineering**: Most production data lakes use Standard or Intelligent-Tiering for active data, Glacier Flexible/Deep Archive for data older than 1-2 years.
