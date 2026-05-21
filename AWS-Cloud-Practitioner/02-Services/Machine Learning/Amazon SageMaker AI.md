# Amazon SageMaker AI

## What you'll learn
- What SageMaker is and who uses it
- The ML workflow SageMaker supports
- SageMaker vs pre-built AI services
- Key exam facts

---

## What is Amazon SageMaker AI?

Amazon SageMaker AI is a **fully managed machine learning platform** for data scientists and ML engineers to build, train, and deploy custom ML models at scale. It provides all the tools needed for the entire ML lifecycle in one integrated environment.

SageMaker is for teams that need to build their own models — not for teams that can use a pre-built AI service (Rekognition, Comprehend, Polly, etc.).

---

## The ML workflow SageMaker supports

**1. Prepare data** — SageMaker Data Wrangler and Feature Store help prepare and store training data.

**2. Build** — SageMaker Studio is a web-based IDE where data scientists write code (Python, Jupyter notebooks) to define their model architecture.

**3. Train** — SageMaker launches managed training jobs on clusters of EC2 instances (including GPU instances). You don't manage the training infrastructure — SageMaker provisions and terminates it automatically.

**4. Tune** — SageMaker Automatic Model Tuning (hyperparameter optimization) runs multiple training jobs to find the best model parameters.

**5. Deploy** — SageMaker deploys trained models to managed endpoints (real-time inference) or batch transform jobs (offline inference). It handles scaling, load balancing, and monitoring.

**6. Monitor** — SageMaker Model Monitor detects model drift — when a deployed model's performance degrades over time.

---

## Key SageMaker components

| Component | Purpose |
|-----------|---------|
| SageMaker Studio | Web-based ML IDE with notebooks |
| SageMaker Training | Managed infrastructure for training jobs |
| SageMaker Endpoints | Real-time model hosting and inference |
| SageMaker Pipelines | ML workflow automation (CI/CD for ML) |
| SageMaker Feature Store | Store and retrieve ML features |
| SageMaker Canvas | No-code ML for business users |

---

## SageMaker vs pre-built AI services

| | SageMaker AI | Pre-built AI services (Rekognition, Comprehend, etc.) |
|-|-------------|------------------------------------------------------|
| Who uses it | Data scientists and ML engineers | Developers (no ML background needed) |
| Models | Custom — you build and train | Pre-trained — call the API |
| Use case | Unique problems requiring custom models | Common AI tasks (vision, NLP, speech) |
| Expertise required | ML knowledge needed | None — just API calls |

---

## When to use it

**Custom ML model** — Your problem is unique and no pre-built AWS AI service covers it — for example, predicting customer churn using your own historical data.

**Fine-tune existing models** — Adapt a foundation model or open-source model to your specific domain.

**Large-scale ML in production** — Train models on hundreds of GPU instances and deploy to auto-scaling endpoints.

---

## Exam focus

- SageMaker = **build, train, and deploy custom ML models** — the full ML platform
- For data scientists, not general developers
- Manages the infrastructure for training and hosting — you focus on the model, not the servers
- Key differentiator from other AI services: SageMaker = **custom models**; Rekognition/Comprehend/Polly = **pre-built models via API**
- Use when exam describes: "train a custom ML model," "data science team," "build a recommendation model on proprietary data"

---

## Practice questions

**Q1.** A data science team wants to build a custom model to predict which loans are likely to default, using the company's proprietary historical loan data. They need to train the model on GPU instances and deploy it to a production endpoint. Which AWS service provides this capability?

A) Amazon Comprehend  
B) Amazon Rekognition  
C) Amazon SageMaker AI  
D) AWS Lambda

**Answer: C** — SageMaker is the ML platform for building, training, and deploying custom models. The team can prepare data, write model code in Studio, train on GPU instances, and deploy to a managed endpoint — all within SageMaker. Comprehend and Rekognition are pre-built AI services for NLP and image analysis — they don't support training on proprietary tabular data. Lambda runs code but is not an ML platform.
