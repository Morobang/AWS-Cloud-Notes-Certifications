# AWS Support

## What you'll learn
- The five AWS Support plan tiers and their key differences
- Response times for each tier
- What a Technical Account Manager (TAM) is
- How Trusted Advisor access differs by plan
- Key exam facts

---

## What is AWS Support?

AWS Support provides access to AWS technical experts and resources to help you use AWS effectively. Support is offered in five tiers, from free basic access to a premium plan with a dedicated technical advisor and 15-minute response to critical outages.

---

## The five support plans

### Basic (Free)
Available to all AWS accounts at no charge.
- Customer service for billing and account questions
- Access to AWS documentation, whitepapers, and the Knowledge Center
- AWS Health Dashboard access (service health and personal alerts)
- 7 core Trusted Advisor checks (security + service limits)
- No technical support for service issues

### Developer ($29/month minimum)
Adds email access to technical support.
- 1 primary contact can submit unlimited support cases
- Business hours email access to Cloud Support Associates
- Response times: 24 business hours (general), 12 business hours (system impaired)
- Still only 7 core Trusted Advisor checks

### Business ($100/month or 10% of monthly bill, whichever is higher)
First plan with 24/7 access and full Trusted Advisor.
- 24/7 phone, email, and chat access to Cloud Support Engineers
- Response times: 24 hours (general), 12 hours (system impaired), 4 hours (production down), **1 hour (business-critical down)**
- **Full Trusted Advisor access** (all 100+ checks)
- AWS Support API for integration with ticketing systems

### Enterprise On-Ramp ($5,500/month minimum)
Adds a pool of TAMs and faster critical response.
- All Business plan features
- Access to a **pool of Technical Account Managers** (shared, not dedicated)
- **30-minute** response for business-critical outages
- Concierge Support Team for billing/account assistance
- Online training and labs

### Enterprise ($15,000/month minimum)
The highest tier — dedicated TAM and 15-minute critical response.
- All Enterprise On-Ramp features
- **Dedicated Technical Account Manager** (one person, your primary AWS contact)
- **15-minute** response for business-critical outages
- Infrastructure Event Management included (for major launches or events)
- Well-Architected Reviews
- White-glove case routing

---

## Support plan comparison

| Feature | Basic | Developer | Business | Ent. On-Ramp | Enterprise |
|---------|-------|-----------|----------|--------------|------------|
| Cost | Free | $29/mo | $100/mo | $5,500/mo | $15,000/mo |
| Technical support | None | Business hours email | 24/7 phone/chat/email | 24/7 phone/chat/email | 24/7 phone/chat/email |
| Critical response | — | — | 1 hour | 30 minutes | **15 minutes** |
| TAM | No | No | No | Pool | **Dedicated** |
| Trusted Advisor | 7 checks | 7 checks | **Full** | **Full** | **Full** |

---

## Technical Account Manager (TAM)

A TAM is a dedicated AWS technical advisor who:
- Develops an ongoing relationship with your team
- Understands your architecture and business goals
- Proactively identifies risks and optimization opportunities
- Acts as your escalation point for critical issues
- Available only on **Enterprise On-Ramp** (shared pool) and **Enterprise** (dedicated)

---

## Trusted Advisor access by plan

| Plan | Trusted Advisor |
|------|----------------|
| Basic | 7 core checks (security + service limits only) |
| Developer | 7 core checks |
| Business | Full access — all 100+ checks across all 5 categories |
| Enterprise On-Ramp | Full access |
| Enterprise | Full access |

The 7 core checks include: MFA on root account, unrestricted security groups, public S3 bucket permissions, EBS/RDS public snapshots, IAM use, service limits.

---

## Exam focus

- Know the **response times** for each plan, especially the critical-down scenarios:
  - Business: 1 hour
  - Enterprise On-Ramp: 30 minutes
  - Enterprise: 15 minutes
- **Full Trusted Advisor** requires **Business or higher**
- **Dedicated TAM** requires **Enterprise** (not On-Ramp — that's a shared pool)
- Developer has **no phone support** and no production-down SLA
- Basic is free but has **no technical support** for service issues

---

## Practice questions

**Q1.** A company runs a mission-critical e-commerce platform. They need a dedicated Technical Account Manager and a 15-minute response time when critical systems go down. Which support plan meets these requirements?

A) Business Support  
B) Enterprise On-Ramp  
C) Enterprise Support  
D) Developer Support

**Answer: C** — Only Enterprise Support provides a *dedicated* TAM (not a shared pool) and a 15-minute response for business-critical outages. Enterprise On-Ramp provides a shared pool of TAMs and 30-minute critical response — both fall short. Business provides 1-hour critical response and no TAM. Developer has no production-down response.

---

**Q2.** A developer on a Basic Support plan wants to use Trusted Advisor to identify all underutilized EC2 instances and unused EBS volumes to reduce costs. Is this possible?

A) Yes — all Trusted Advisor checks are available on all plans  
B) No — cost optimization checks require Business Support or higher  
C) Yes — cost checks are included in the 7 core checks  
D) No — Trusted Advisor is only available on Enterprise Support

**Answer: B** — Basic and Developer plans provide only 7 core Trusted Advisor checks focused on security and service limits. Cost optimization checks (idle EC2 instances, unattached EBS volumes) require Business Support or higher, which provides access to all 100+ checks across all five categories.
