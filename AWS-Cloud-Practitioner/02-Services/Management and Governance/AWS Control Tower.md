# AWS Control Tower

## What you'll learn
- What Control Tower does and what problem it solves
- Landing zones and guardrails
- Control Tower vs AWS Organizations
- Key exam facts

---

## What is AWS Control Tower?

AWS Control Tower is a service that **sets up and governs a secure, compliant multi-account AWS environment** — called a landing zone — using best practices from AWS. It automates the creation of a well-architected multi-account structure so organizations don't have to manually configure it.

---

## The problem it solves

Large organizations need multiple AWS accounts (dev, staging, prod, security, audit, networking) for isolation and governance. Setting this up correctly — with consolidated billing, centralized logging, guardrails, and access management — requires expertise and many manual steps.

Control Tower automates this entire setup in a few clicks.

---

## Key concepts

**Landing zone** — The overall multi-account AWS environment that Control Tower creates and manages. Includes a management account, a log archive account, and an audit account by default.

**Organizational Units (OUs)** — Logical groupings of accounts under AWS Organizations. Control Tower creates a predefined OU structure (Sandbox, Production, etc.) that you can customize.

**Guardrails** — Governance policies that apply to accounts in your landing zone. Two types:
- **Preventive guardrails** — SCPs (Service Control Policies) that prevent accounts from taking certain actions. Example: "Prevent users from disabling CloudTrail."
- **Detective guardrails** — AWS Config rules that detect non-compliant configurations and alert. Example: "Detect if an account has root account MFA disabled."

**Account Factory** — A self-service portal for creating new AWS accounts that automatically comply with your landing zone standards. When a new team needs an account, they request it through Account Factory; the account is created with all guardrails pre-applied.

---

## Control Tower vs AWS Organizations

| | AWS Control Tower | AWS Organizations |
|-|------------------|-------------------|
| Scope | Full landing zone setup — accounts, guardrails, logging | Account structure and SCPs only |
| Automation | Automated best-practice setup | Manual — you build the structure |
| Guardrails | Pre-built preventive and detective guardrails | SCPs only (preventive) |
| Best for | Greenfield: setting up multi-account from scratch | Already have Organizations; add SCPs |

Control Tower uses AWS Organizations under the hood — it adds automation, Account Factory, and detective guardrails on top.

---

## When to use it

**New multi-account setup** — An organization wants to adopt AWS with a well-governed multi-account structure from the start.

**Regulated industries** — Quickly establish a compliant environment with logging, audit trails, and guardrails that meet regulatory requirements.

---

## Exam focus

- Control Tower = **automated multi-account landing zone** — sets up governance, guardrails, and account structure
- **Guardrails** = governance policies (preventive SCPs + detective Config rules)
- **Account Factory** = self-service new account creation with guardrails pre-applied
- Uses **AWS Organizations** as the foundation
- Use when exam describes: "multi-account governance," "landing zone," "set up multiple accounts with best practices," "account vending machine"

---

## Practice questions

**Q1.** A company is starting their cloud journey and wants to set up a multi-account AWS environment with pre-built governance controls, centralized logging, and a self-service way for teams to request new accounts. Which AWS service provides this?

A) AWS IAM Identity Center  
B) AWS Organizations  
C) AWS Control Tower  
D) AWS Service Catalog

**Answer: C** — Control Tower automates the setup of a well-governed multi-account environment (landing zone) with guardrails, centralized logging, and Account Factory for self-service account provisioning. Organizations provides the account structure but requires manual configuration. IAM Identity Center manages single sign-on access. Service Catalog manages pre-approved product provisioning.
