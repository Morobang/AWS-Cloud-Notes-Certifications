# Amazon Q

## What you'll learn
- What Amazon Q is and the two main products
- Amazon Q Business vs Amazon Q Developer
- Key use cases
- Key exam facts

---

## What is Amazon Q?

Amazon Q is AWS's **generative AI assistant** — a family of AI-powered products that help employees and developers get answers, generate content, and automate tasks using large language models (LLMs).

Amazon Q has two main products:

**Amazon Q Business** — An AI assistant for business users. It connects to company data sources and lets employees ask questions in natural language and get answers grounded in company information.

**Amazon Q Developer** — An AI coding assistant integrated into IDEs (VS Code, JetBrains). It helps developers write code, explain code, find bugs, and answer AWS-related questions.

---

## Amazon Q Business

Amazon Q Business connects to your organization's data — SharePoint, S3, Confluence, Salesforce, ServiceNow, Jira, and more — and lets employees ask questions like:

- "What is our refund policy?"
- "Summarize last quarter's sales report"
- "Who is the contact for the Acme account?"

Q Business retrieves information from the connected sources and generates a natural-language answer. It respects existing access controls — users only see answers from documents they're authorized to read.

**Administrators** can configure guardrails (topics Q should not discuss), customize the interface, and connect data sources through the Q Business console.

---

## Amazon Q Developer

Amazon Q Developer is an AI coding assistant that:

- Generates code from natural language descriptions
- Autocompletes code in the IDE
- Explains complex code snippets
- Identifies security vulnerabilities
- Answers questions about AWS services and how to use them
- Can perform multi-step tasks like scanning a codebase for security issues

It is integrated into VS Code, JetBrains IDEs, the AWS Console, and the AWS CLI.

---

## Amazon Q vs Amazon Kendra

| | Amazon Q Business | Amazon Kendra |
|-|------------------|---------------|
| Output | Generative AI answers | Specific document passages |
| Technology | LLM-based generation | ML-based retrieval |
| Interaction | Conversational | Search query |
| Best for | AI assistant for employees | Precise document search |

Both connect to enterprise data sources, but Q generates conversational answers while Kendra retrieves specific matching passages.

---

## When to use it

**Employee productivity** — Let employees ask business questions without digging through documents and systems.

**Developer assistance** — Accelerate software development with AI code generation and AWS guidance.

**IT support** — Employees ask IT questions; Q answers from knowledge base articles automatically.

---

## Exam focus

- Amazon Q = **generative AI assistant** for business users and developers
- **Q Business** = AI assistant connected to company data sources for employee Q&A
- **Q Developer** = AI coding assistant in the IDE (generates code, explains code, AWS guidance)
- Use when exam describes: "AI assistant for employees," "answer questions from company documents," "AI coding helper"

---

## Practice questions

**Q1.** A company wants to give employees an AI-powered assistant that can answer questions about internal policies, project status, and customer data by searching across connected data sources including SharePoint and Salesforce. Which AWS service enables this?

A) Amazon Kendra  
B) Amazon Q Business  
C) Amazon Comprehend  
D) Amazon SageMaker AI

**Answer: B** — Amazon Q Business is a generative AI assistant that connects to enterprise data sources and provides conversational answers to employee questions. Kendra provides ML-powered search and returns document passages — it does not generate conversational answers. Comprehend extracts insights from text but doesn't answer questions. SageMaker is a platform for building custom ML models.
