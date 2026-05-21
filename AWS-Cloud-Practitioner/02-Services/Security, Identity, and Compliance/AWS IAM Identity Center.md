# AWS IAM Identity Center

## What you'll learn
- What IAM Identity Center is and what problem it solves
- Single sign-on across accounts and applications
- IAM Identity Center vs IAM
- Key exam facts

---

## What is AWS IAM Identity Center?

AWS IAM Identity Center (formerly AWS SSO) is a **centralized service for managing single sign-on (SSO) access to multiple AWS accounts and cloud applications**. Instead of creating separate IAM users in each account, employees log in once and get access to all their authorized accounts and apps.

---

## The problem without IAM Identity Center

A company with 20 AWS accounts would need to:
- Create IAM users in each account (or use complex cross-account role assumptions)
- Manage separate passwords for each account
- Onboard and offboard users in 20 places
- Audit access across 20 separate accounts

IAM Identity Center solves this by centralizing identity management.

---

## How it works

1. Connect IAM Identity Center to your identity source:
   - **IAM Identity Center directory** (built-in user directory)
   - **Active Directory** (via AWS Directory Service or a managed AD)
   - **External IdP** (Okta, Azure AD, Ping, etc.) via SAML 2.0

2. Assign users/groups to AWS accounts with specific permission sets

3. Users log in at a single portal URL — they see all accounts and applications they're authorized for

4. Clicking an account opens the AWS console for that account — with appropriate permissions — without separate login

---

## Permission sets

A **permission set** is a collection of policies that define what a user can do in an AWS account. You create permission sets once and assign them to users/groups for specific accounts.

Examples: `AdministratorAccess`, `ReadOnly`, `DeveloperAccess` — applied to different accounts for different user groups.

---

## IAM Identity Center vs IAM

| | IAM Identity Center | IAM |
|-|--------------------|----|
| Scope | Multiple accounts, external apps | Single account |
| Users | Managed centrally or from external IdP | Managed per account |
| Access | SSO — one login for all | Separate credentials per account |
| Best for | Multi-account organizations | Single-account or programmatic access |

---

## When to use it

**Multi-account organizations** — Grant employees access to the right accounts with the right permissions from a central portal.

**External identity integration** — Let employees use their existing corporate credentials (Azure AD, Okta) to access AWS.

**Simplified offboarding** — Remove a user from IAM Identity Center; they immediately lose access to all AWS accounts.

---

## Exam focus

- IAM Identity Center = **SSO for AWS accounts and applications** — one login, access to all
- Integrates with **Active Directory and external IdPs** (Okta, Azure AD)
- Uses **permission sets** to define what users can do in each account
- Key differentiator from IAM: IAM Identity Center is for centralized multi-account access; IAM manages within a single account

---

## Practice questions

**Q1.** A company with 30 AWS accounts wants employees to log in once using their existing corporate Active Directory credentials and get access to all authorized accounts without separate passwords for each. Which AWS service enables this?

A) AWS IAM  
B) AWS IAM Identity Center  
C) Amazon Cognito  
D) AWS Directory Service

**Answer: B** — IAM Identity Center provides SSO across all AWS accounts, integrates with Active Directory, and presents a unified portal for all authorized accounts and applications. IAM manages access within a single account — not across 30. Cognito is for app user authentication (web/mobile users). Directory Service provides AD in AWS but doesn't provide the SSO portal or multi-account access management.
