# Amazon Connect

## What you'll learn
- What Amazon Connect is and what it replaces
- Key capabilities of Connect
- How Connect integrates with other AWS services
- Key exam facts

---

## What is Amazon Connect?

Amazon Connect is a **cloud-based contact center service**. Companies use it to set up and manage customer service call centers — handling inbound and outbound phone calls, live chat, and automated IVR (interactive voice response) flows — without the need for traditional on-premises telephony hardware.

You pay per minute of usage rather than purchasing expensive hardware or per-seat licenses.

---

## Key capabilities

- **Omnichannel contact center**: Voice (phone), chat, and task management from a single platform
- **Visual call flow designer**: Drag-and-drop interface for building IVR menus and call routing logic (no coding required)
- **Agent workspace**: Web-based interface for agents to handle calls, chats, and view customer information
- **Real-time and historical metrics**: Dashboards showing agent performance, queue wait times, and call volumes
- **Contact Lens**: Built-in ML-powered conversation analytics — sentiment analysis, keyword detection, and call summarization
- **Forecasting and scheduling**: AI-based workforce management tools

---

## Integration with other AWS services

| Integration | What it enables |
|-------------|----------------|
| Amazon Lex | Add AI-powered IVR chatbots to call flows — customers can speak or type naturally to resolve issues without an agent |
| Amazon Transcribe | Automatically transcribe call recordings for compliance and analysis |
| Amazon Comprehend | Analyze call transcripts for sentiment and key topics |
| Lambda | Run custom logic during call flows (look up customer data, update CRM records) |
| S3 | Store call recordings |
| DynamoDB | Look up customer profiles during calls |

---

## When to use it

**Migrating a call center to the cloud** — Replace legacy on-premises PBX systems with a cloud contact center that scales automatically.

**Remote agent workforce** — Agents work from anywhere using just a browser and headset — no physical office required.

**AI-powered IVR** — Use Lex chatbots to handle common inquiries (order status, account balance) automatically, routing to a human agent only for complex issues.

**Compliance and analytics** — Contact Lens automatically transcribes and analyzes calls for quality assurance and compliance monitoring.

---

## Exam focus

- Connect = **cloud-based contact center** — phone/chat/IVR for customer service
- Pay-per-minute pricing — no hardware or per-seat licenses
- Integrates with **Amazon Lex** for AI-powered conversational IVR
- **Contact Lens** provides ML-based call analytics (sentiment, transcription, keyword detection)
- Use when the exam mentions: "contact center," "call center," "IVR," "customer service agents," "cloud phone system"

---

## Practice questions

**Q1.** A company wants to migrate their on-premises call center to AWS. They need agents to handle inbound customer calls from home, with an AI-powered IVR that can answer common questions automatically before routing to a human agent. Which AWS service provides this?

A) Amazon Chime  
B) Amazon Connect  
C) Amazon Lex  
D) Amazon Transcribe

**Answer: B** — Amazon Connect is the cloud-based contact center that provides the full call routing, IVR, and agent workspace. Amazon Lex (used for the AI-powered IVR) integrates within Connect call flows, but Connect is the overall contact center platform. Amazon Transcribe converts speech to text. Amazon Chime is a video conferencing and messaging tool.
