# AWS Identity and Access Management (IAM)

## What you'll learn
- What IAM is and its core components
- Users, groups, roles, and policies
- Key security principles: least privilege, MFA, root account
- Key exam facts

---

## What is AWS IAM?

AWS Identity and Access Management (IAM) is the **service that controls who can access AWS resources and what they can do**. It is free and available in every AWS account. IAM is a global service — it is not Region-specific.

---

## Core components

**IAM User** — An identity representing a person or application that interacts with AWS. A user has:
- Credentials: password (for console) and/or access keys (for CLI/SDK)
- Permissions: attached directly or via groups

**IAM Group** — A collection of IAM users. Assign policies to the group; all members inherit those permissions. Simplifies permission management.

**IAM Role** — An identity with permissions that is assumed temporarily — by an AWS service (EC2, Lambda), another account, or a federated user. Unlike users, roles don't have permanent credentials — they issue short-lived temporary credentials.

**IAM Policy** — A JSON document defining permissions (allow or deny actions on resources). Attached to users, groups, or roles.

---

## Policy structure

An IAM policy document has:
- **Effect**: Allow or Deny
- **Action**: Which API actions (e.g., `s3:GetObject`, `ec2:*`)
- **Resource**: Which resources the policy applies to (e.g., specific S3 bucket ARN or `*` for all)

```json
{
  "Effect": "Allow",
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::my-bucket/*"
}
```

---

## Key security principles

**Principle of least privilege** — Grant only the permissions needed to perform a task, nothing more.

**Root account** — The original account owner with unrestricted access. Best practices:
- Never use the root account for daily operations
- Enable MFA on the root account immediately
- Create an IAM admin user for day-to-day management

**Multi-Factor Authentication (MFA)** — Require a second factor (authenticator app, hardware token) in addition to a password. Enable MFA on the root account and privileged IAM users.

**Password policy** — Enforce minimum length, complexity, and rotation requirements for IAM user passwords.

---

## IAM Roles for services

When an EC2 instance needs to access S3, don't give it a user's access keys. Instead:
1. Create an IAM role with the required S3 permissions
2. Attach the role to the EC2 instance
3. The instance's code automatically gets temporary credentials through the instance metadata service

This avoids storing long-lived credentials on the instance.

---

## Exam focus

- IAM = **identity and access control** — users, groups, roles, policies
- **Least privilege**: grant only what's needed
- **Root account**: never use for daily tasks; enable MFA
- **IAM Roles**: preferred over access keys for EC2/Lambda accessing other services
- **MFA**: extra security layer for console and API access
- IAM is **global** — not Region-specific

---

## Practice questions

**Q1.** A company wants to ensure their EC2 instances can access Amazon S3 without storing AWS access keys on the instances. Which approach follows AWS security best practices?

A) Store access keys in environment variables on each EC2 instance  
B) Hardcode access keys in the application code  
C) Attach an IAM role with the required S3 permissions to the EC2 instance  
D) Create an IAM user with S3 permissions and share the credentials with the instances

**Answer: C** — IAM roles provide temporary credentials automatically rotated by AWS — no need to store access keys on the instance. EC2 instances use the instance metadata service to retrieve temporary credentials from the attached role. All other options require storing long-lived credentials, which is a security risk.

**Q2.** A new employee needs to access AWS resources. Which IAM entity should be created for them?

A) IAM Root User  
B) IAM Role  
C) IAM User  
D) IAM Policy

**Answer: C** — Create an IAM User for each person who needs to access AWS. Add them to appropriate groups to assign permissions. The root user should not be used for individual employees. Roles are assumed temporarily, not for individual daily access. Policies define permissions but aren't identities themselves.
