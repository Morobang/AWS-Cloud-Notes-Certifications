# Domain 4: Data Security and Governance — Practice Questions

> 15 questions | ~18% of the DEA-C01 exam

---

**Q1.** A company needs to ensure analysts can query a customer table in Athena but cannot see the `ssn`, `credit_card`, and `bank_account` columns. What is the MOST appropriate solution?

A) Create separate S3 buckets without those columns and point analysts to those buckets  
B) Create separate IAM roles that deny access to specific columns  
C) Use AWS Lake Formation to grant column-level SELECT permissions on only the allowed columns  
D) Use Amazon Macie to automatically block PII columns in queries

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

AWS Lake Formation supports column-level security — you can grant a principal SELECT access to specific columns only. Athena (and other Lake Formation-integrated services) enforces this at query time.

- A creates data duplication and maintenance burden.
- B — IAM cannot restrict at the column level.
- D — Macie discovers PII but doesn't restrict query access.
</details>

---

**Q2.** A data team is storing patient health records in Amazon S3. Auditors require proof of who accessed which data, with the ability to trace every read operation. What combination of services meets this requirement?

A) Enable S3 server access logging + CloudWatch  
B) Enable CloudTrail data events on the S3 bucket + integrate with CloudWatch Logs  
C) Enable Amazon Macie on the S3 bucket  
D) Use VPC Flow Logs to track access to S3

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

CloudTrail data events (object-level events) record every `GetObject`, `PutObject`, `DeleteObject` call — who made it, from what IP, at what time, with what IAM credentials. Sending to CloudWatch Logs enables alerting and analysis.

- A (Server access logs) also captures S3 requests but with less detail than CloudTrail and not all metadata.
- C (Macie) finds PII but doesn't audit access.
- D (VPC Flow Logs) captures network traffic, not application-level S3 operations.
</details>

---

**Q3.** A company uses SSE-S3 for encrypting their data lake in Amazon S3. Their compliance team says they need to prove to auditors that encryption keys are being used appropriately and be able to revoke access to data. What should they switch to?

A) SSE-C (Customer-Provided Keys)  
B) SSE-KMS with Customer Managed Keys  
C) Client-side encryption  
D) Keep SSE-S3 but enable CloudTrail

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

SSE-KMS with Customer Managed Keys (CMK) provides:
- **Audit trail**: Every KMS key use appears in CloudTrail
- **Revocation**: Disable or delete the KMS key → data becomes inaccessible
- **Access control**: Specific IAM roles can use the key; others cannot

SSE-S3 uses AWS-managed keys with no customer visibility or control.
</details>

---

**Q4.** A South African fintech company stores customer financial data in S3. Under POPIA, they must be able to honor "right to erasure" requests — permanently deleting a specific customer's data. The data is stored in large Parquet files with millions of records each. What is the MOST practical approach?

A) Delete the entire Parquet file containing the customer's record  
B) Use Amazon Macie to automatically delete PII  
C) Tokenize the customer's PII at ingestion using a key stored in AWS KMS; delete the key for that customer to render their data inaccessible  
D) Recreate the Parquet file without the customer's record every time an erasure request comes in

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Deleting a specific record from a Parquet file requires rewriting the entire file — impractical at petabyte scale. The cryptographic erasure approach:
1. At ingestion, encrypt PII for each customer with a unique key stored in KMS
2. Upon erasure request: delete the KMS key
3. The customer's data in S3 is now permanently unreadable — crypto-erased

This is a compliance-recognized approach that avoids the cost of rewriting all files.

- A loses all other customers' data.
- B — Macie discovers PII, doesn't erase it.
- D is technically correct but operationally impossible at scale.
</details>

---

**Q5.** A company has a data lake in Account A. A data science team in Account B needs to run Athena queries against tables in Account A's Glue Data Catalog. What is the correct way to grant this access?

A) Share the S3 bucket URL with the data scientists and have them create their own Glue tables  
B) Use AWS Lake Formation cross-account sharing via AWS RAM (Resource Access Manager)  
C) Create IAM users in Account A and share credentials with Account B users  
D) Copy all data to Account B's S3 bucket

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Lake Formation cross-account sharing: Account A grants Lake Formation permissions on tables to Account B's IAM role. Account B accepts the RAM share. Account B users can now query Account A's data through their own Athena without data being copied.

- A creates schema management complexity — no single source of truth.
- C sharing credentials is a security violation.
- D creates data duplication, stale copies, and storage costs.
</details>

---

**Q6.** A Glue ETL job that writes to S3 is failing with an access denied error. The IAM role for the Glue job has S3 PutObject permissions. What else could be causing the failure?

A) The IAM role needs Glue administrator permissions  
B) The S3 bucket has an S3 bucket policy that denies the Glue role  
C) Glue jobs cannot write to S3 — they can only read  
D) The data format must be CSV for Glue to write to S3

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Access to S3 is the intersection of IAM policies AND S3 bucket policies AND (if applicable) Lake Formation permissions. Even if the IAM role allows `s3:PutObject`, an explicit Deny in the S3 bucket policy will override it. Always check both the IAM policy AND the resource policy on the target.
</details>

---

**Q7.** A company needs to enforce that all new S3 objects in their data lake must be encrypted with SSE-KMS. Unencrypted PutObject requests should be rejected. How do you implement this?

A) Enable default bucket encryption — this automatically rejects unencrypted uploads  
B) Create an S3 bucket policy with a Deny statement on PutObject requests that lack the `s3:x-amz-server-side-encryption` header or specify a non-KMS encryption type  
C) Configure CloudTrail to alert on unencrypted uploads  
D) Use Amazon Inspector to scan for unencrypted objects

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

A bucket policy can DENY `s3:PutObject` requests that don't include the correct encryption header:

```json
{
  "Effect": "Deny",
  "Action": "s3:PutObject",
  "Resource": "arn:aws:s3:::my-bucket/*",
  "Condition": {
    "StringNotEquals": {
      "s3:x-amz-server-side-encryption": "aws:kms"
    }
  }
}
```

Note: Default bucket encryption (A) applies encryption to requests that don't specify it, but doesn't REJECT them. The bucket policy Deny is required to actually block non-KMS uploads.
</details>

---

**Q8.** A data governance team wants to automatically scan new data uploaded to S3 for personally identifiable information (PII) and receive alerts when PII is found in unexpected buckets. Which service handles this?

A) AWS Config  
B) Amazon GuardDuty  
C) Amazon Macie  
D) AWS CloudTrail

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Amazon Macie uses ML to automatically discover and classify sensitive data in S3 — names, SSNs, credit card numbers, passport numbers, bank account numbers, and more. It creates findings that can trigger EventBridge rules → SNS alerts.

- A (Config) monitors resource configuration, not data content.
- B (GuardDuty) detects threats from API behavior patterns, not data content.
- D (CloudTrail) logs API calls, not data content.
</details>

---

**Q9.** Which type of encryption provides the ability to encrypt individual columns in a Redshift table with different encryption keys, so that different teams can only decrypt the columns they're authorized to see?

A) Redshift cluster-level encryption with KMS  
B) Column-level encryption using UDFs and AWS KMS  
C) Lake Formation column-level security  
D) VPC encryption for Redshift traffic

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Redshift supports column-level encryption using User-Defined Functions (UDFs) that call AWS KMS. You can encrypt specific columns with specific keys, and only users/roles that can call `decrypt()` with the right KMS key can read the plaintext.

Note: Lake Formation column-level security (C) is for Glue Data Catalog/Athena, not Redshift native tables.
</details>

---

**Q10.** A company's data pipeline writes customer PII to an S3 bucket. Under their data governance policy, PII must be masked before it reaches the analytics layer. At which pipeline layer should masking be applied?

A) At the Bronze layer (raw ingestion) — never store PII even raw  
B) During the Bronze → Silver transformation — mask PII before it lands in Silver  
C) Only at the Gold layer — analysts' final dataset  
D) PII masking happens automatically in Amazon Athena

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

The most common pattern: keep raw data (including PII) in the Bronze layer with strict access controls (only ingestion processes can read it), then mask/tokenize PII during the Bronze → Silver transformation. The Silver layer and beyond have no PII.

This balances: (A) you still have original data for audit/compliance, (B) analytics layers are clean.

If regulations require never storing PII at all, then A is correct — but this makes data lineage and correction harder.
</details>

---

**Q11.** A data engineer is designing IAM policies for a team of Glue ETL developers. The principle of least privilege requires that they can develop and run Glue jobs but cannot modify the Glue Data Catalog schemas used by production Athena queries. What is the BEST approach?

A) Give developers the `AWSGlueConsoleFullAccess` managed policy  
B) Create a custom IAM policy that allows `glue:StartJobRun`, `glue:GetJob`, and `glue:CreateJob`, but explicitly denies `glue:UpdateTable` and `glue:DeleteTable` on production databases  
C) Create separate AWS accounts for development and production  
D) Use S3 bucket policies to prevent Glue from modifying the Data Catalog

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Least privilege: grant exactly what developers need (run jobs, create jobs, view job status) while denying destructive catalog operations on production. Custom IAM policies with explicit Deny on production resource ARNs is the right pattern.

- A gives full console access — too broad.
- C works but is overkill for this requirement.
- D — S3 policies don't govern Glue Data Catalog operations.
</details>

---

**Q12.** What does DynamoDB Time To Live (TTL) accomplish for data governance?

A) Automatically archives old data to S3 Glacier  
B) Deletes expired items from the table automatically without consuming write capacity  
C) Encrypts old items with a new key after a specified period  
D) Moves old items to a cold storage tier within DynamoDB

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

DynamoDB TTL: set a Unix timestamp in an attribute. DynamoDB automatically deletes items after that timestamp passes — at no write capacity cost. Items are deleted within ~48 hours of expiry.

Used for: session tokens, temporary data, event logs with short retention requirements.

This helps comply with data minimization principles — don't keep data longer than needed.
</details>

---

**Q13.** A data lake needs to comply with regulations requiring that certain data cannot be modified or deleted for 7 years. How can this be implemented in Amazon S3?

A) Enable S3 Versioning and set lifecycle rules to expire after 7 years  
B) Enable S3 Object Lock in Compliance mode with a 7-year retention period  
C) Use S3 Intelligent-Tiering to automatically protect old objects  
D) Apply an S3 bucket policy that denies DeleteObject for all users

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

S3 Object Lock in Compliance mode creates WORM (Write Once Read Many) storage:
- Objects cannot be deleted or overwritten during the retention period
- Even the root account and AWS cannot override Compliance mode
- Perfect for regulatory requirements like SEC Rule 17a-4

Governance mode also locks but allows the root account to override.

- A doesn't prevent deletion — someone can delete all versions.
- D can be bypassed by the bucket owner changing the policy.
</details>

---

**Q14.** A company uses AWS Lake Formation for their data lake. An analyst complains that despite having the correct IAM permissions to access the Glue Data Catalog, they're getting "Access Denied" when querying with Athena. What is the MOST likely cause?

A) The analyst's IAM role has not been added to the Athena workgroup  
B) Lake Formation permissions have not been granted to the analyst's IAM role for the specific table or database  
C) The S3 bucket policy is blocking the analyst  
D) The Glue Data Catalog is encrypted and the analyst doesn't have KMS access

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

When Lake Formation is enabled, BOTH IAM permissions AND Lake Formation permissions must allow access. Having IAM access to the Glue Data Catalog is not sufficient — Lake Formation acts as a second gate. The analyst's role needs explicit `SELECT` permission granted in Lake Formation on the specific database/table.

This is the most common misconfiguration when organizations first adopt Lake Formation.
</details>

---

**Q15.** A company is building a data pipeline where EMR Spark jobs read from and write to S3. They want to prevent EMR from reading data in certain sensitive S3 prefixes. What is the BEST approach with Lake Formation?

A) Configure VPC endpoint policies to restrict EMR's S3 access  
B) Register sensitive S3 locations with Lake Formation and grant only the allowed prefixes to the EMR service role  
C) Use AWS Config rules to prevent EMR from accessing certain S3 paths  
D) Add EMR to a security group that blocks access to S3

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Lake Formation with the EMR integration: register S3 paths as data lake locations and control access via Lake Formation permissions. The EMR service role is a principal in Lake Formation — you grant it access only to the allowed locations and tables, denying the sensitive prefixes.

Lake Formation + EMR uses the LF credential vending mechanism to provide temporary credentials scoped to allowed locations only.
</details>
