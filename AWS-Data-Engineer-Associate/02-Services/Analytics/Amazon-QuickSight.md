# Amazon QuickSight

**Category**: Business Intelligence and Visualization  
**Exam weight**: Medium — know SPICE, row-level security, and integration with Athena/Redshift

---

## What Is It?

Amazon QuickSight is AWS's fully managed Business Intelligence (BI) service for creating interactive dashboards, visualizations, and ad-hoc analysis. It connects to Athena, Redshift, S3, RDS, and many other sources.

---

## Key Architecture Components

### SPICE (Super-fast Parallel In-memory Calculation Engine)

SPICE is QuickSight's in-memory query engine. When you import data into SPICE:
- Data is stored in QuickSight's managed infrastructure
- Queries execute in milliseconds regardless of source query complexity
- All concurrent users query SPICE, not the source database
- Reduces load on Redshift/Athena/RDS

**SPICE capacity**: Each account gets 10 GB of SPICE. Purchase additional capacity per GB-month.

**Refresh**: SPICE data must be refreshed to stay current. Configure hourly, daily, or weekly refresh schedules — or trigger refreshes via API.

**Direct query mode**: Instead of importing to SPICE, QuickSight queries the source directly on each dashboard load. Results are live but slower. Use for low-traffic dashboards with real-time data requirements.

---

## Data Sources

| Source | SPICE | Direct query |
|---|---|---|
| Amazon Athena | Yes | Yes |
| Amazon Redshift | Yes | Yes |
| Amazon S3 (CSV/JSON) | Yes | No |
| Amazon RDS / Aurora | Yes | Yes |
| Amazon OpenSearch | No | Yes |
| Salesforce, ServiceNow | Yes | No |
| JDBC (on-premises) | Yes | No |

---

## Access Control

### Row-Level Security (RLS)

Restrict which rows each user or group sees in a dataset.

**How to configure:**
1. Create a permissions dataset (a table mapping user/group to allowed values)
2. Attach the permissions dataset to your data dataset
3. QuickSight automatically applies the filter based on the logged-in user

```
Permissions table:
| UserName     | region |
|--------------|--------|
| alice@co.com | EMEA   |
| bob@co.com   | APAC   |

Data table: sales (has 'region' column)

When alice queries → QuickSight adds WHERE region = 'EMEA' automatically
```

### Column-Level Security

Hide specific columns from certain groups — prevent analysts from seeing PII columns in a dataset.

### Sharing and Permissions

- **Readers**: Can view dashboards, filtered by RLS. Pay-per-session pricing (max $5/user/month for 30 sessions)
- **Authors**: Can create and edit analyses and dashboards
- **Admins**: Manage users, data sources, SPICE

**Sharing dashboards**: Publish a dashboard → share with specific users or groups → optionally embed

---

## Embedding

QuickSight dashboards can be embedded in external applications:

**Embedded dashboard**: Full interactive dashboard inside your web app. Users authenticate via your app; QuickSight generates a signed URL.

**Embedded visual**: Single chart widget embedded in your app.

Use case: SaaS products where each tenant sees their own data (combined with RLS).

```python
import boto3
quicksight = boto3.client('quicksight', region_name='us-east-1')

response = quicksight.generate_embed_url_for_registered_user(
    AwsAccountId='123456789012',
    ExperienceConfiguration={
        'Dashboard': {
            'InitialDashboardId': 'my-dashboard-id'
        }
    },
    UserArn='arn:aws:quicksight:us-east-1:123456789012:user/default/alice'
)

embed_url = response['EmbedUrl']
```

---

## ML Insights

Built-in ML features requiring no data science expertise:

**Anomaly Detection**: Automatically identifies unusual patterns in time-series data. "Alert when daily sales drop more than 20% below the expected range."

**Forecasting**: Predicts future values of a metric using ML-based time-series forecasting. Add a forecast line to any time-series chart.

**Auto-narratives**: Generates natural language summaries of charts ("Sales increased 15% week over week, driven by the APAC region.").

---

## QuickSight Q (Natural Language Querying)

Type a question in plain English to get instant visualizations:

"Show me monthly revenue by product category for 2024"

QuickSight Q uses NLP to interpret the question, generates a SQL query, and renders the appropriate chart type.

**Setup**: Define a "topic" that maps business terms to dataset fields ("revenue" = `order_amount`, "product category" = `category_name`).

---

## QuickSight + Athena Pattern

The most common data lake analytics pattern:

```
S3 data lake (Parquet, partitioned)
    ↓
Glue Data Catalog (schema)
    ↓
Athena (SQL engine)
    ↓
QuickSight dataset (imports via Athena query into SPICE)
    ↓
Dashboard (shared with business users)
```

Analysts build dashboards without knowing Athena or SQL — they interact with QuickSight's point-and-click interface.

---

## Exam Scenarios

| Scenario | Answer |
|---|---|
| 500 business users need dashboards from Athena data without each running SQL | QuickSight with SPICE import — users query SPICE, not Athena |
| Regional managers should see only their region's data in the same dashboard | QuickSight Row-Level Security |
| Embed a per-customer analytics dashboard inside a SaaS web application | QuickSight Embedded Dashboards with RLS per tenant |
| Data needs to be no more than 1 hour old in dashboards | SPICE with hourly refresh schedule (or direct query for real-time) |
| Anomaly in sales data should automatically trigger an alert | QuickSight Anomaly Detection + alert notification |
