# Amazon S3 Glacier

## What you'll learn
- What S3 Glacier is and who uses it
- The three Glacier storage tiers
- Retrieval options and their trade-offs
- Key exam facts

---

## What is Amazon S3 Glacier?

Amazon S3 Glacier is a **low-cost S3 storage class designed for long-term data archival and backup**. It is not a separate service — Glacier storage classes are part of Amazon S3. Data stored in Glacier is designed to be retained for months to years and is rarely accessed.

The trade-off: very low storage cost, but retrieval takes time (and incurs a retrieval fee).

---

## The three Glacier storage tiers

**S3 Glacier Instant Retrieval**
- Retrieval time: **milliseconds** (same as S3 Standard)
- Cost: Lower than S3 Standard; lower than S3 Standard-IA
- Use case: Archive data that is occasionally accessed (once per quarter) but must be retrieved immediately when needed
- Minimum storage duration: 90 days

**S3 Glacier Flexible Retrieval** (formerly S3 Glacier)
- Retrieval time: **1–5 minutes** (Expedited), **3–5 hours** (Standard), **5–12 hours** (Bulk — free)
- Cost: Ultra-low storage cost
- Use case: Archive data accessed once or twice per year; time to retrieve is acceptable
- Minimum storage duration: 90 days

**S3 Glacier Deep Archive**
- Retrieval time: **12 hours** (Standard), **48 hours** (Bulk)
- Cost: Lowest of all S3 storage classes
- Use case: Long-term regulatory or compliance archives — digital preservation, financial records, medical records
- Minimum storage duration: 180 days

---

## Glacier vs S3 Standard

| | S3 Standard | S3 Glacier Flexible | S3 Glacier Deep Archive |
|-|------------|--------------------|-----------------------|
| Storage cost | Highest | Low | Lowest |
| Retrieval time | Immediate | 1 min – 12 hours | 12 – 48 hours |
| Retrieval cost | None | Per GB (except bulk) | Per GB |
| Use case | Active data | Archive, rare access | Long-term compliance |

---

## Vault Lock

For strict compliance requirements, **S3 Glacier Vault Lock** allows you to set a compliance policy that cannot be changed — once locked, data cannot be deleted for the specified retention period (write-once, read-many / WORM).

---

## When to use it

**Regulatory compliance archives** — Healthcare records (HIPAA), financial records (SEC Rule 17a-4), and legal documents that must be retained for 7–30 years at minimal cost.

**Media and entertainment archives** — Store completed film and television productions, raw footage, and master files indefinitely.

**Backup retention** — Keep database backups beyond the standard 35-day RDS backup window.

---

## Exam focus

- S3 Glacier = **ultra-low-cost archival storage** within S3
- Three tiers: **Instant** (milliseconds) → **Flexible** (minutes to hours) → **Deep Archive** (hours to days)
- Cost decreases as retrieval time increases
- **Vault Lock** = WORM compliance (cannot be deleted within retention period)
- Use when exam describes: "long-term archive," "compliance retention," "lowest cost storage," "rarely accessed data"

---

## Practice questions

**Q1.** A financial institution must retain transaction records for 10 years to meet regulatory requirements. The records are never accessed after 6 months but must be retrievable within 48 hours if auditors request them. Which S3 storage class is most cost-effective?

A) S3 Standard  
B) S3 Standard-IA  
C) S3 Glacier Flexible Retrieval  
D) S3 Glacier Deep Archive

**Answer: D** — Glacier Deep Archive offers the lowest storage cost with retrieval within 12–48 hours — meeting the 48-hour retrieval requirement at the absolute minimum cost. S3 Standard and Standard-IA are designed for more frequent access and cost more. Glacier Flexible Retrieval is cheaper than Standard-IA but more expensive than Deep Archive for 10-year retention.
