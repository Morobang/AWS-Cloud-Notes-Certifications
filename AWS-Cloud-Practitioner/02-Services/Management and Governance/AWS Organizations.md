# AWS Organizations

## What you'll learn
- What AWS Organizations is and why multi-account matters
- Organizational Units (OUs) and account structure
- Service Control Policies (SCPs)
- Consolidated billing
- Key exam facts

---

## What is AWS Organizations?

AWS Organizations is a service that **lets you centrally manage and govern multiple AWS accounts**. It provides a management hierarchy, centralized billing, and policy-based controls across all accounts in your organization.

---

## Why use multiple accounts?

A single AWS account for everything creates risk:
- A misconfigured IAM policy in one project could accidentally affect another
- There's no natural isolation between development and production
- Billing is one big lump — hard to allocate costs by team

**Multiple accounts** provide:
- **Isolation**: A security incident in one account doesn't automatically affect others
- **Cost visibility**: Each account's spend is separately tracked
- **Permission boundaries**: IAM in one account doesn't affect another
- **Service limits**: Separate limits per account (EC2 vCPU limits, etc.)

---

## Key concepts

**Management account** (formerly master account) — The root account that creates the organization and manages all member accounts. Holds the overall billing.

**Member accounts** — AWS accounts that belong to the organization. Managed by the management account.

**Organizational Units (OUs)** — Logical groupings of accounts within the organization hierarchy. Example structure:
- Root
  - Sandbox OU (developer experiments)
  - Production OU
    - App A account
    - App B account
  - Security OU (centralized logging, audit)

Policies applied to an OU cascade down to all accounts within it.

---

## Service Control Policies (SCPs)

SCPs are **IAM-like permission policies applied at the OU or account level** that act as permission guardrails — defining the maximum permissions any IAM entity in that account can have.

SCPs don't grant permissions — they restrict them. Even if an IAM user in an account has `AdministratorAccess`, an SCP on the account can prevent specific actions.

Example SCPs:
- "No accounts in this OU can disable CloudTrail"
- "Accounts in the Sandbox OU cannot launch resources outside of us-east-1"
- "No account can leave the organization"

---

## Consolidated billing

All member account charges roll up to the management account for a single bill. Benefits:
- One invoice for the entire organization
- **Volume discounts**: Usage across all accounts is aggregated — you reach higher discount tiers faster
- **Reserved Instance and Savings Plans sharing**: Unused RI capacity in one account can be used by other accounts in the organization

---

## When to use it

**Multi-account strategy** — Organize accounts by environment (dev/staging/prod), by business unit, or by application.

**Centralized governance** — Use SCPs to enforce security policies across all accounts (e.g., prevent disabling security services).

**Cost management** — Consolidated billing with volume discounts and centralized cost allocation tags.

---

## Exam focus

- Organizations = **multi-account management** — centralized billing, OUs, SCPs
- **SCPs** = permission guardrails on OUs and accounts (restrict max permissions)
- **Consolidated billing** = one invoice, volume discounts, RI sharing
- **OUs** = logical groupings of accounts for applying policies
- Use when exam describes: "manage multiple accounts," "central billing," "prevent accounts from disabling security services," "apply policies across accounts"

---

## Practice questions

**Q1.** A company uses 15 AWS accounts for different teams and environments. They want to ensure no account in the "Development" organizational unit can launch resources in regions outside of us-east-1, regardless of what IAM permissions those accounts have. Which Organizations feature enforces this restriction?

A) IAM permission boundaries  
B) AWS Config rules  
C) Service Control Policies (SCPs)  
D) AWS Control Tower guardrails

**Answer: C** — SCPs set the maximum permissions for all accounts in an OU. A deny SCP on the Development OU that restricts resource creation outside us-east-1 will apply regardless of the IAM policies in those accounts. IAM permission boundaries apply within a single account. Config rules detect compliance violations but can't prevent actions. Control Tower guardrails use SCPs under the hood — C is the more direct answer here.
