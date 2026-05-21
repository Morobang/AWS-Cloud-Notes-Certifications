# Task 4.1: Apply Authentication and Authorization

## Learning Objectives
- Configure IAM roles and policies for data services
- Apply Lake Formation permissions for fine-grained data access control
- Implement resource-based policies on S3 buckets and Glue Data Catalog
- Design cross-account data access patterns

---

## IAM for Data Services

### Service Roles vs User Credentials

Data pipelines should never use long-term IAM user credentials (access keys). Instead, use IAM roles assumed by the service:

| Service | IAM mechanism |
|---|---|
| Glue ETL job | IAM role attached to the job — Glue assumes it to access S3, Data Catalog, CloudWatch |
| Lambda function | Lambda execution role |
| EMR cluster | EC2 instance profile on cluster nodes |
| Redshift COPY (S3 load) | IAM role attached to the Redshift cluster |
| CodeBuild | IAM service role for CodeBuild |
| ECS/EKS tasks | Task execution role |

**Minimum permissions principle for Glue:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:DeleteObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::my-data-lake",
        "arn:aws:s3:::my-data-lake/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": ["glue:*"],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "cloudwatch:PutMetricData",
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "*"
    }
  ]
}
```

---

## S3 Bucket Policies

S3 bucket policies are resource-based policies — attached to the bucket itself.

### Enforce HTTPS Only

```json
{
  "Effect": "Deny",
  "Principal": "*",
  "Action": "s3:*",
  "Resource": ["arn:aws:s3:::my-bucket", "arn:aws:s3:::my-bucket/*"],
  "Condition": {
    "Bool": {"aws:SecureTransport": "false"}
  }
}
```

### Restrict Access to Specific VPC

```json
{
  "Effect": "Deny",
  "Principal": "*",
  "Action": "s3:*",
  "Resource": "arn:aws:s3:::my-bucket/*",
  "Condition": {
    "StringNotEquals": {
      "aws:SourceVpc": "vpc-12345678"
    }
  }
}
```

### Cross-Account S3 Access

Bucket policy granting another account read access:
```json
{
  "Effect": "Allow",
  "Principal": {"AWS": "arn:aws:iam::ACCOUNT-B:role/DataTeamRole"},
  "Action": ["s3:GetObject", "s3:ListBucket"],
  "Resource": ["arn:aws:s3:::my-bucket", "arn:aws:s3:::my-bucket/*"]
}
```

Account B's role also needs an IAM policy allowing it to access Account A's bucket.

---

## AWS Lake Formation — Fine-Grained Access Control

### The Problem Lake Formation Solves

Without Lake Formation, controlling who sees what in a data lake requires:
- S3 bucket policies (coarse — bucket or prefix level)
- IAM policies per user per table
- No concept of column-level or row-level permissions

With Lake Formation, you grant permissions at the table, column, or row level — one place, applied consistently to Athena, EMR, Glue, and Redshift Spectrum.

### How Lake Formation Works

1. Register S3 locations with Lake Formation (LF takes control of access)
2. Revoke default `IAMAllowedPrincipals` grants (required — otherwise IAM policies bypass LF)
3. Grant permissions using Lake Formation instead of IAM/S3 policies

### Grant Types

**Database/Table permissions:**
```python
import boto3
lf = boto3.client('lakeformation')

# Grant SELECT on a table to an IAM role
lf.grant_permissions(
    Principal={'DataLakePrincipalIdentifier': 'arn:aws:iam::123456789:role/AnalystRole'},
    Resource={
        'Table': {
            'DatabaseName': 'sales_db',
            'Name': 'fact_orders'
        }
    },
    Permissions=['SELECT']
)
```

**Column-level security:**
```python
# Grant access to specific columns only (hide PII)
lf.grant_permissions(
    Principal={'DataLakePrincipalIdentifier': 'arn:aws:iam::123456789:role/AnalystRole'},
    Resource={
        'TableWithColumns': {
            'DatabaseName': 'customer_db',
            'Name': 'customers',
            'ColumnNames': ['customer_id', 'region', 'segment']
            # Note: 'ssn' and 'email' are NOT listed → not accessible
        }
    },
    Permissions=['SELECT']
)
```

**Row-level filters:**
Create a data filter that restricts rows, then grant access with that filter applied:
```python
# Create filter: only rows where region = 'ZA'
lf.create_data_cells_filter(
    TableData={
        'TableCatalogId': '123456789',
        'DatabaseName': 'sales_db',
        'TableName': 'orders',
        'Name': 'za_region_filter',
        'RowFilter': {
            'FilterExpression': "region = 'ZA'"
        },
        'ColumnWildcard': {}  # All columns
    }
)
```

### Tag-Based Access Control (LF-TBAC)

Instead of granting per-table, assign metadata tags to resources and grant access to tag values:

```
Tag: sensitivity=public, sensitivity=confidential

AnalystRole → granted access to sensitivity=public
DataEngineerRole → granted access to sensitivity=public AND sensitivity=confidential
```

When a new table is created and tagged `sensitivity=public`, AnalystRole automatically gets access — no new grants needed.

---

## Glue Data Catalog Resource Policies

The Glue Data Catalog has its own resource policy — who can access the catalog metadata:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {"AWS": "arn:aws:iam::ACCOUNT-B:root"},
      "Action": [
        "glue:GetDatabase",
        "glue:GetTable",
        "glue:GetTables",
        "glue:SearchTables"
      ],
      "Resource": [
        "arn:aws:glue:us-east-1:ACCOUNT-A:catalog",
        "arn:aws:glue:us-east-1:ACCOUNT-A:database/my_db",
        "arn:aws:glue:us-east-1:ACCOUNT-A:table/my_db/*"
      ]
    }
  ]
}
```

Cross-account Athena queries use the source account's catalog — grant both catalog access and S3 bucket access to the querying account.

---

## VPC Endpoints for Data Services

Data services accessed from within a VPC should use VPC endpoints to avoid internet traffic:

| Service | Endpoint type |
|---|---|
| Amazon S3 | Gateway endpoint (free, route-table based) |
| Amazon DynamoDB | Gateway endpoint (free) |
| AWS Glue | Interface endpoint (PrivateLink, costs per hour) |
| Amazon Kinesis | Interface endpoint |
| AWS Secrets Manager | Interface endpoint |

A Gateway endpoint adds a route to your VPC route table. An Interface endpoint creates an ENI in your VPC subnet.

---

## Cross-Account Data Access Patterns

### Centralized Data Lake Pattern

Accounts: Producers (team accounts) → Central Data Lake Account (S3 + Lake Formation) → Consumers (analytics team)

1. Each producer account assumes a role in the central account to write data to S3
2. Lake Formation governs read access in the central account
3. Consumer accounts are granted Lake Formation table/column permissions

### AWS RAM (Resource Access Manager)

Share AWS resources across accounts without copying:
- Share Glue Data Catalog databases with other accounts (cross-account table access in Athena)
- Share VPC subnets with other accounts

### S3 Access Points

Create named access points per team or use case, each with its own access policy:

```
S3 Bucket: company-data-lake
  ├── Access Point: analysts-ap (allows SELECT from analytics IAM roles, restricted to /silver/ prefix)
  ├── Access Point: ml-team-ap  (allows all operations on /gold/ prefix)
  └── Access Point: public-ap   (read-only on /public/ prefix)
```

Each access point has its own VPC or internet-facing configuration. Simplifies managing large numbers of access patterns on one bucket.

---

## Redshift Access Control

### Database Users and Groups

Redshift has its own user model separate from IAM:
```sql
-- Create a database user
CREATE USER analyst_alice PASSWORD 'Temp1234!';

-- Create a group and add user
CREATE GROUP analysts;
ALTER GROUP analysts ADD USER analyst_alice;

-- Grant SELECT on schema to group
GRANT SELECT ON ALL TABLES IN SCHEMA public TO GROUP analysts;
```

### Redshift IAM Integration

For programmatic access, use `GetClusterCredentials` to generate temporary database credentials from an IAM role — no static passwords.

### Redshift Row-Level Security

```sql
-- Create a policy that filters rows by region
CREATE RLS POLICY region_policy
USING (region = current_user);

-- Attach to table
ATTACH RLS POLICY region_policy ON sales FOR ROLE analysts;
```

---

## Exam Scenarios

**"A Glue ETL job needs to read from an S3 bucket and write results to another bucket. The job is currently failing with AccessDenied."**
→ The Glue service role is missing `s3:GetObject` on the source bucket and/or `s3:PutObject` on the destination bucket — update the IAM policy

**"Analysts should be able to query the customer table in Athena but must not see the SSN or credit card number columns."**
→ Lake Formation column-level permissions — grant SELECT on the table but exclude those columns

**"Data engineers in Account B need to query tables in the Glue Data Catalog in Account A using Athena."**
→ Cross-account: Account A adds Glue resource policy + S3 bucket policy granting Account B's role access; Account B's IAM role needs permission to query Athena and access the catalog

**"Each analyst should see only data from their own region in the sales dashboard."**
→ Lake Formation row-level filter per user, OR Redshift RLS policy if using Redshift
