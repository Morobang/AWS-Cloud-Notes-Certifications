# Amazon Kendra

## What you'll learn
- What enterprise search is and what problem it solves
- What Amazon Kendra does
- How Kendra differs from traditional keyword search
- Key exam facts

---

## What is Amazon Kendra?

Amazon Kendra is a **fully managed intelligent enterprise search service** powered by machine learning. It allows employees to search across all of an organization's internal content — SharePoint, S3 documents, Confluence, ServiceNow, databases, and more — and get relevant, natural-language answers rather than a list of links.

---

## The problem with traditional enterprise search

Traditional search returns a ranked list of documents that contain the keywords you typed. Finding the actual answer still requires opening documents and reading them.

Kendra understands the meaning of the question and returns the specific passage or paragraph that answers it — like asking a knowledgeable colleague instead of searching a filing cabinet.

**Example**: Query: "What is the company's parental leave policy?"

- Traditional search: returns 12 documents mentioning "parental leave"
- Kendra: returns the exact passage from the HR handbook that states the policy

---

## How Kendra works

1. You connect Kendra to your data sources (S3, SharePoint, Salesforce, ServiceNow, Confluence, databases, etc.) using pre-built connectors
2. Kendra crawls and indexes the content
3. Users type natural language questions
4. Kendra returns the most relevant answer, with the source document cited

---

## Key features

- **Natural language understanding**: Understands questions, not just keywords
- **30+ data source connectors**: Connect to common enterprise content repositories out of the box
- **Relevance tuning**: Adjust how Kendra ranks results based on your business needs
- **Incremental learning**: Kendra learns from user feedback over time to improve results
- **Access control**: Respects document-level permissions — users only see documents they're authorized to access

---

## When to use it

**Internal knowledge base** — Allow employees to find answers in company wikis, HR documentation, IT runbooks, and policy documents.

**Customer support** — Power a support portal where customers can ask questions and get answers from product documentation.

**Legal and compliance** — Let legal teams search across contracts and regulatory documents to find relevant clauses.

---

## Exam focus

- Kendra = **intelligent enterprise search** — natural language search across internal documents
- Uses ML to return **specific answers**, not just document links
- Connects to 30+ enterprise data sources (SharePoint, S3, Confluence, ServiceNow)
- Use when exam describes: "search internal documents," "employees need to find information across multiple systems," "natural language search"

---

## Practice questions

**Q1.** A large organization stores HR policies, IT runbooks, and company wikis across SharePoint, S3, and Confluence. Employees struggle to find specific answers because they have to search multiple systems and read through documents. Which AWS service provides a unified intelligent search experience?

A) Amazon Comprehend  
B) Amazon OpenSearch Service  
C) Amazon Kendra  
D) Amazon Rekognition

**Answer: C** — Kendra provides intelligent enterprise search across multiple data sources, returning specific natural-language answers rather than document lists. Comprehend extracts insights from text (sentiment, entities) but doesn't provide a search interface. OpenSearch is a general-purpose search and analytics engine — it returns keyword matches, not ML-powered answers. Rekognition analyzes images and video.
