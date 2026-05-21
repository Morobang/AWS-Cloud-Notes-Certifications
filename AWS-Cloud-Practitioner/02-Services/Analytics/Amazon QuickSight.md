# Amazon QuickSight

## What you'll learn
- What QuickSight is and who it's for
- How QuickSight connects to data sources
- What SPICE is
- Key exam facts about QuickSight

---

## What is Amazon QuickSight?

Amazon QuickSight is AWS's **business intelligence (BI) service**. It lets you connect to data sources, create interactive charts and dashboards, and share them with business stakeholders — without writing code or managing infrastructure.

QuickSight is the AWS alternative to tools like Tableau, Power BI, and Looker.

---

## Key features

- **Broad data source connectivity**: Connects to Amazon Redshift, RDS, Aurora, S3, Athena, DynamoDB, OpenSearch, Salesforce, Excel/CSV files, and more
- **Interactive dashboards**: Drag-and-drop chart builder; supports bar charts, line charts, pie charts, maps, pivot tables, KPIs, and more
- **SPICE**: QuickSight's in-memory data engine that caches imported data for fast query performance without hitting the source system every time
- **ML Insights**: Built-in anomaly detection and forecasting, surfaced directly in dashboards
- **Shareable and embeddable**: Dashboards can be shared by email or embedded in applications
- **Pay-per-session pricing**: Business users are billed per 15-minute session, cost-effective for large numbers of occasional users

---

## What is SPICE?

**SPICE** (Super-fast, Parallel, In-memory Calculation Engine) is QuickSight's data caching layer. When you import data into QuickSight (rather than running a live query), SPICE stores it in memory for very fast dashboard loading times.

- SPICE queries are fast and don't add load to source systems
- Each QuickSight user gets a SPICE storage allocation
- Data must be refreshed (manually or on a schedule) when the source changes

---

## When to use it

**Executive dashboards** — Sales data from Redshift visualized as monthly revenue charts shared with leadership.

**Self-service analytics** — Business analysts build their own reports from approved data sources without asking engineering.

**Embedded analytics** — A SaaS platform embeds QuickSight dashboards to show customers their usage metrics.

**Cross-source reporting** — Combining data from Salesforce, S3, and RDS into a single dashboard.

---

## Exam focus

- QuickSight = **business intelligence dashboards** — the AWS BI/visualization tool
- Connects to Redshift, Athena, S3, RDS, and many other sources
- **SPICE** = in-memory cache for fast dashboard performance
- Pay-per-session pricing — cost-effective for infrequent users
- Use when the exam mentions: "dashboards," "visualizations," "business reports," "BI tool," "share charts with stakeholders"
- Not a database or query engine — it visualizes data, not stores it

---

## Practice questions

**Q1.** A retail company wants to give regional managers interactive dashboards showing daily sales and inventory trends. The data lives in Amazon Redshift. Which service should they use to build these dashboards?

A) Amazon Athena  
B) Amazon QuickSight  
C) AWS Glue  
D) Amazon Kinesis

**Answer: B** — QuickSight is the AWS BI service for creating and sharing interactive dashboards. It connects natively to Redshift and provides the chart-building tools that non-technical managers need. Athena is a query tool, not a BI tool. Glue is for ETL. Kinesis is for streaming data ingestion.
