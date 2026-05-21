# AWS Secrets Manager

## What you'll learn
- What Secrets Manager stores and why it exists
- Automatic secret rotation
- Secrets Manager vs Parameter Store
- Key exam facts

---

## What is AWS Secrets Manager?

AWS Secrets Manager is a service for **securely storing, managing, and automatically rotating secrets** — database credentials, API keys, OAuth tokens, and other sensitive configuration values. Applications retrieve secrets from Secrets Manager at runtime, instead of having them hardcoded or stored in config files.

---

## The problem it solves

Hardcoding database passwords in application code or config files is a security risk:
- The password is stored in plaintext in version control or on the filesystem
- Rotating passwords requires code deployments
- Multiple services sharing one credential is difficult to manage

Secrets Manager provides a secure, auditable, centrally managed store for secrets — and can rotate them automatically.

---

## How it works

1. Store a secret in Secrets Manager (e.g., an RDS username and password)
2. Grant your application's IAM role permission to retrieve the secret
3. Your application calls the Secrets Manager API to retrieve the secret at runtime
4. Secrets Manager returns the current value
5. The secret is never stored in code, config files, or environment variables

---

## Automatic secret rotation

Secrets Manager can automatically rotate secrets on a schedule:
- For supported AWS services (RDS, Redshift, DocumentDB), Secrets Manager rotates the database password automatically and updates the secret value — your application always gets the current password without changes
- Uses a Lambda function to perform the rotation
- You can define rotation schedules (e.g., every 30, 60, or 90 days)

---

## Secrets Manager vs SSM Parameter Store

| | Secrets Manager | SSM Parameter Store |
|-|----------------|---------------------|
| Primary purpose | Secrets with auto-rotation | Configuration data and secrets |
| Automatic rotation | Yes (built-in for RDS, etc.) | No (manual) |
| Cost | Per secret per month | Free for standard parameters |
| Secrets? | Yes — designed for secrets | Yes — SecureString with KMS |
| Best for | Credentials that rotate | Config values, feature flags |

**Use Secrets Manager when** you need automatic rotation. Use Parameter Store for config values that don't rotate.

---

## When to use it

**Database credentials** — Store and rotate RDS/Aurora passwords automatically. Applications retrieve the current password from Secrets Manager at startup.

**API keys** — Store third-party API keys securely; retrieve them at runtime.

**Multi-account secret sharing** — Share secrets across multiple AWS accounts.

---

## Exam focus

- Secrets Manager = **store and auto-rotate secrets** (database passwords, API keys)
- **Automatic rotation** is the key differentiator from Parameter Store
- Applications retrieve secrets via API at runtime — no hardcoded credentials
- Use when exam describes: "rotate database passwords automatically," "store credentials securely," "retrieve secrets at runtime"

---

## Practice questions

**Q1.** A company wants to automatically rotate their RDS database passwords every 30 days. Applications should always connect to the database using the current password without requiring redeployment. Which AWS service provides this?

A) AWS Systems Manager Parameter Store  
B) AWS Key Management Service  
C) AWS Secrets Manager  
D) AWS IAM

**Answer: C** — Secrets Manager stores the RDS credentials and automatically rotates the password on a defined schedule. The application retrieves the current password from Secrets Manager at runtime — no redeployment needed when the password rotates. Parameter Store can store secrets but does not have built-in automatic rotation. KMS manages encryption keys, not application secrets. IAM doesn't store or rotate database credentials.
