# Task Statement 3.7: Identify AWS AI/ML and Analytics Services

## What you'll learn
- The AWS AI/ML services and what problem each one solves
- The key analytics services and when to use each
- How to identify the right service from a scenario description

---

## AI and Machine Learning services

The exam tests service selection — you don't need to know how to build models. For each service, the key question is: "What does this service do, and when would you use it?"

### Amazon SageMaker

SageMaker is the platform for building, training, and deploying machine learning models. It's the end-to-end tool for data scientists and ML engineers who want to build custom models.

SageMaker manages the infrastructure for every stage of the ML lifecycle: preparing data, training models on GPU clusters, evaluating performance, and deploying models as prediction endpoints.

*Use when*: You need a custom model trained on your own data. Everything else in this list (Rekognition, Comprehend, etc.) is a pre-built model you call via API. SageMaker is for building your own.

---

### Amazon Rekognition

Rekognition analyzes images and videos using pre-built computer vision models. You send it an image or video; it tells you what's in it.

**Capabilities**:
- Object and scene detection ("this image contains a dog, grass, and a park")
- Facial detection and analysis (age range, emotions, glasses, face comparison)
- Text detection in images (OCR)
- Unsafe content detection (explicit content moderation)
- Celebrity recognition
- Video analysis: activity detection, people counting

*Use when*: Moderating user-uploaded content, verifying identity via face comparison, analyzing security camera footage, searching video by object or face.

---

### Amazon Comprehend

Comprehend is a natural language processing (NLP) service. It reads text and extracts meaning and structure.

**Capabilities**:
- **Sentiment analysis**: Is this customer review positive, negative, or neutral?
- **Entity recognition**: Identifies people, places, organizations, dates, and amounts in text
- **Key phrase extraction**: What are the important phrases in this document?
- **Language detection**: What language is this text written in?
- **PII detection**: Does this text contain names, SSNs, credit card numbers?

*Use when*: Analyzing customer reviews, support tickets, or social media posts at scale. Categorizing documents. Detecting sensitive information in text.

---

### Amazon Translate

Translate provides real-time machine translation between languages. It currently supports over 75 languages.

*Use when*: Building multilingual applications, translating customer support content, localizing product listings, processing documents in multiple languages.

---

### Amazon Lex

Lex is the service for building conversational interfaces — chatbots and voice assistants. It's the technology behind Alexa.

**What it handles**: Understanding natural language intent ("I want to check my order status"), extracting parameters ("Order number 12345"), and managing multi-turn conversation flow.

*Use when*: Building customer service chatbots, automated IVR (interactive voice response) systems, internal IT helpdesk bots.

---

### Amazon Polly

Polly converts text to speech. You send it text; it returns an audio file.

*Use when*: Adding voice to applications, generating audio versions of articles, accessibility features, voice-based notifications.

---

### Amazon Transcribe

Transcribe converts speech to text — the opposite of Polly. It processes audio or video files and returns a text transcript.

**Features**: Speaker identification (who said what), custom vocabulary (for industry-specific terms), live transcription.

*Use when*: Transcribing call center recordings for analysis, generating meeting notes, creating closed captions, processing voice memos.

---

### Amazon Textract

Textract extracts text and data from scanned documents — PDFs, images of forms, tables, and handwriting. Beyond OCR, it understands document structure: it can identify form fields and values ("Name: John Smith"), table rows and columns.

*Use when*: Processing invoices, extracting data from contracts, digitizing medical records, automating form processing.

---

### Amazon Forecast

Forecast creates time-series forecasting models. You provide historical data (sales history, website traffic, inventory levels) and Forecast predicts future values.

*Use when*: Demand forecasting, inventory planning, financial projections, energy demand prediction.

---

### Amazon Kendra

Kendra is an intelligent enterprise search service. It uses ML to search documents and return the specific answer to a question — not just a list of documents, but the actual answer extracted from them.

*Use when*: Building an internal search tool for company documents, knowledge bases, policy repositories. Users ask "what is the vacation policy?" and get the answer directly from the relevant document.

---

### Amazon Personalize

Personalize creates real-time personalization and recommendation models. You provide behavioral data (what users clicked, purchased, or watched); Personalize trains and deploys a recommendation model.

*Use when*: "Customers who bought this also bought..." product recommendations, personalized content feeds, targeted email campaigns.

---

## AI service quick reference

| Service | Input | Output | Use case |
|---------|-------|--------|----------|
| SageMaker | Your data | Custom ML model | Build custom ML models |
| Rekognition | Image/video | Labels, faces, text | Content moderation, identity verification |
| Comprehend | Text | Sentiment, entities, language | Analyze customer feedback |
| Translate | Text | Translated text | Multilingual apps |
| Lex | Text/voice | Conversation response | Chatbots |
| Polly | Text | Audio | Text-to-speech |
| Transcribe | Audio | Text transcript | Speech-to-text |
| Textract | Document/image | Structured text and data | Form/invoice processing |
| Forecast | Historical data | Future predictions | Demand forecasting |
| Kendra | Question + documents | Direct answer | Enterprise search |
| Personalize | User behavior data | Recommendations | Product suggestions |

---

## Analytics services

### Amazon Athena

Athena runs SQL queries directly against data in S3 without loading it into a database first. There's no infrastructure to manage — you pay only for the data scanned by each query.

**The key feature**: Query data where it lives in S3. No ETL, no data loading, no cluster to manage.

*Use when*: Ad-hoc analysis of log files, querying CSV/JSON/Parquet data in S3, quick exploration of datasets before building a full data pipeline.

---

### AWS Glue

Glue is a serverless ETL (Extract, Transform, Load) service. ETL is the process of extracting data from one or more sources, transforming it into the right shape, and loading it into a destination.

**Components**:
- **Glue Crawlers**: Automatically scan data sources and build a data catalog (metadata about what's stored where and in what format)
- **Glue Jobs**: Serverless Spark/Python jobs that transform and move data
- **Glue Data Catalog**: Central metadata repository used by Athena, Redshift, and other services

*Use when*: Building data pipelines that move and transform data between S3, Redshift, RDS, and other stores.

---

### Amazon Kinesis

Kinesis is a suite of services for processing real-time streaming data — data that arrives continuously and must be processed immediately or near-immediately.

**Kinesis Data Streams**: Ingest and store high-volume data streams in real time. Multiple consumers can process the same stream simultaneously. You build the processing logic.

**Kinesis Data Firehose**: Simplest way to load streaming data into S3, Redshift, or OpenSearch. No code required — configure the destination, and Firehose buffers and delivers the data automatically.

**Kinesis Data Analytics**: Run SQL or Apache Flink queries against streaming data in real time.

*Use when*: Clickstream analysis, real-time log monitoring, IoT sensor data processing, fraud detection that must act on data within seconds of arrival.

---

### Amazon QuickSight

QuickSight is AWS's business intelligence (BI) tool. It connects to data sources (Redshift, RDS, S3, Athena, Salesforce, Excel files) and creates interactive dashboards and visualizations.

*Use when*: Building dashboards for business metrics, sharing charts and graphs with stakeholders, replacing tools like Tableau or Power BI.

---

### Amazon EMR — Elastic MapReduce

EMR is a managed big data platform that runs Apache Hadoop, Spark, Hive, Presto, and other open-source big data frameworks. It provisions and manages the cluster, handles setup and configuration, and scales the cluster as needed.

*Use when*: Large-scale data processing using existing Spark or Hadoop workloads, ETL at massive scale, machine learning on big data.

---

### AWS Lake Formation

Lake Formation makes it faster to build a data lake — a centralized repository that stores structured and unstructured data at any scale. It automates much of the setup work: ingesting data, cataloging it, and configuring security and access controls.

*Use when*: Building a data lake on S3, centralizing data from multiple sources, managing access controls for data across your organization.

---

### Amazon OpenSearch Service

OpenSearch (formerly Elasticsearch Service) is a managed search and analytics service. It's a near-real-time search engine for log data, metrics, and text.

*Use when*: Full-text search for applications, analyzing and visualizing log data (with Kibana/OpenSearch Dashboards), security analytics.

---

## Analytics service selection

| Need | Service |
|------|---------|
| Query S3 data with SQL, no setup | Athena |
| Move and transform data (ETL) | AWS Glue |
| Real-time data streaming and processing | Kinesis |
| Business intelligence dashboards | QuickSight |
| Big data with Spark/Hadoop | EMR |
| Build and manage a data lake | Lake Formation |
| Search and log analytics | OpenSearch Service |

---

## Practice questions

**Q1.** A company's customer service team receives thousands of support emails daily. They want to automatically categorize each email as positive, negative, or neutral, and identify which product the customer is referring to. Which service handles this?

A) Amazon Rekognition  
B) Amazon Comprehend  
C) Amazon Lex  
D) Amazon Textract

**Answer: B** — Comprehend performs sentiment analysis (positive/negative/neutral) and entity extraction (identifying products, people, places in text). Rekognition analyzes images and video. Lex builds conversational interfaces. Textract extracts data from scanned documents — not from plain text emails.

---

**Q2.** A retail company stores 3 years of sales transaction logs in S3 as CSV files. A data analyst needs to run occasional SQL queries to investigate trends. They don't want to set up a database or ETL pipeline. Which service is most appropriate?

A) Amazon Redshift  
B) AWS Glue  
C) Amazon Athena  
D) Amazon EMR

**Answer: C** — Athena runs SQL directly against S3 data with no setup, no cluster, and no ETL. You pay only for data scanned. Redshift requires loading data into a cluster. Glue is for building ETL pipelines. EMR requires provisioning a cluster. For ad-hoc analysis of data already in S3, Athena is the simplest and most cost-effective choice.

---

**Q3.** A fintech company needs to detect fraudulent transactions in real time — analysis must complete within 200 milliseconds of a transaction occurring. Which service enables real-time stream processing?

A) Amazon Athena  
B) AWS Glue  
C) Amazon Kinesis  
D) Amazon QuickSight

**Answer: C** — Kinesis ingests and processes data streams in real time — milliseconds to seconds of latency. Kinesis Data Analytics can run queries against the live stream. Athena and Glue process data in batches, not in real time. QuickSight is a BI visualization tool.

---

**Q4.** A company wants to add "customers who bought this also bought" product recommendations to their e-commerce website. The recommendations should be personalized per user based on their browsing and purchase history. Which service provides this?

A) Amazon Kendra  
B) Amazon Forecast  
C) Amazon Personalize  
D) Amazon SageMaker

**Answer: C** — Personalize is built specifically for recommendation systems. It trains on user interaction data (clicks, purchases, ratings) and delivers real-time personalized recommendations via API. Kendra is enterprise document search. Forecast predicts future time-series values. SageMaker could build a custom recommendation model, but Personalize is the purpose-built managed service for this exact use case.

---

**Domain 3 complete.** You now know the key services across compute, databases, networking, storage, AI/ML, and analytics.

**Next:** [Domain 4 — Billing, Pricing, and Support](../../Domain-4-Billing%20Pricing%20and%20Support/README.md)
