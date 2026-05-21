# Amazon Comprehend

## What you'll learn
- What natural language processing (NLP) is
- What Comprehend extracts from text
- Key use cases
- Key exam facts

---

## What is Amazon Comprehend?

Amazon Comprehend is a **fully managed natural language processing (NLP) service**. It uses machine learning to find meaning and insights in unstructured text — without you needing to build or train any ML models.

You provide text; Comprehend returns structured information about what the text says and means.

---

## What Comprehend can extract

| Feature | What it returns | Example |
|---------|----------------|---------|
| Sentiment analysis | Positive, negative, neutral, or mixed | "I love this product!" → Positive |
| Entity recognition | People, organizations, locations, dates, quantities | "Amazon was founded in Seattle in 1994" → [Amazon: ORG], [Seattle: LOCATION], [1994: DATE] |
| Key phrase extraction | The main topics or subjects | "The new iPhone has a better camera" → "new iPhone," "better camera" |
| Language detection | What language the text is written in | "Bonjour" → French |
| PII detection | Personal identifiable information | Detect and redact SSNs, email addresses, phone numbers |
| Topic modeling | Group documents by common themes | Cluster 10,000 support tickets into topics |

---

## How to use it

Comprehend is a simple API — you call it with your text and it returns structured results. No ML training required. AWS has pre-trained the models.

**Comprehend Custom** — Train your own text classifier or entity recognizer on your domain-specific data (e.g., classify insurance claims into categories).

---

## When to use it

**Customer feedback analysis** — Analyze thousands of product reviews or support tickets to identify sentiment trends and common issues.

**Document classification** — Automatically categorize incoming emails, support tickets, or news articles.

**PII redaction** — Detect and remove personally identifiable information from documents before storage or sharing.

**Content insights** — Extract entities, topics, and relationships from large document sets (legal contracts, research papers).

---

## Exam focus

- Comprehend = **NLP service** — extracts sentiment, entities, key phrases, and language from text
- No ML expertise required — call the API with text, get structured results
- **Sentiment analysis** is the most exam-relevant feature: determine if text is positive, negative, or neutral
- Use when exam describes: "analyze customer reviews," "extract information from text," "detect sentiment in feedback"

---

## Practice questions

**Q1.** A retail company receives thousands of product reviews every day and wants to automatically identify whether customers are satisfied or dissatisfied with their purchases. Which AWS service can analyze the text of each review and return a sentiment score?

A) Amazon Rekognition  
B) Amazon Comprehend  
C) Amazon Textract  
D) Amazon Translate

**Answer: B** — Comprehend performs sentiment analysis on text — returning whether the review is positive, negative, neutral, or mixed. Rekognition analyzes images and video. Textract extracts text from document images (e.g., scanned forms). Translate converts text between languages but does not analyze meaning.
