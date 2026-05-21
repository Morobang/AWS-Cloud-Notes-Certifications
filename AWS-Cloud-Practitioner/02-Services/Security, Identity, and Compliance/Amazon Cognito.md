# Amazon Cognito

## What you'll learn
- What Cognito is and who it's for
- User Pools vs Identity Pools
- Cognito vs IAM Identity Center
- Key exam facts

---

## What is Amazon Cognito?

Amazon Cognito is an **identity service for web and mobile application users**. It provides user sign-up, sign-in, and access control for your customer-facing applications — without you building an authentication system from scratch.

Cognito is for your **app's end users** (customers, external users). IAM is for your team and AWS resources.

---

## Two core components

**User Pool** — A user directory for your application:
- Handles user sign-up and sign-in (username/password, email/phone verification)
- Supports social login: users can sign in with Google, Facebook, Apple, or Amazon
- Supports SAML/OIDC federation with enterprise identity providers
- Returns JWT tokens after successful authentication
- Think of it as: "the place where your app's users register and log in"

**Identity Pool** (Federated Identity) — Grants authenticated users temporary AWS credentials to access AWS services:
- Users authenticate through a User Pool (or external IdP)
- Identity Pool maps their identity to an IAM role
- Returns temporary AWS credentials
- Think of it as: "gives my app users permission to access S3, DynamoDB, etc. directly from their browser or mobile device"

---

## Combined flow

A common pattern: User Pool + Identity Pool together:
1. User signs in through User Pool (gets JWT)
2. App exchanges JWT with Identity Pool for temporary AWS credentials
3. App uses credentials to access S3, DynamoDB, etc. directly

---

## Cognito vs IAM Identity Center

| | Amazon Cognito | IAM Identity Center |
|-|---------------|---------------------|
| Users | External app users (customers) | Internal employees |
| Purpose | App authentication and authorization | SSO to AWS accounts and enterprise apps |
| Use case | "Sign up for my mobile app" | "Log in to manage AWS accounts" |

---

## When to use it

**Web/mobile app sign-in** — Let users register and sign in to your application with email/password or social login.

**Federated identity** — Let enterprise users sign in to your app using their corporate credentials.

**Mobile backend access** — Grant authenticated mobile users temporary AWS credentials to access backend resources.

---

## Exam focus

- Cognito = **identity for app users** — sign-up, sign-in, social login, federated access
- **User Pool** = user directory with authentication
- **Identity Pool** = grants temporary AWS credentials to authenticated users
- Key differentiator: Cognito is for your app's end users; IAM Identity Center is for employees accessing AWS accounts

---

## Practice questions

**Q1.** A startup is building a mobile app and wants users to sign in with their email or Google account. After signing in, users should be able to upload photos directly to an S3 bucket. Which AWS service handles both the user authentication and the temporary AWS credentials for S3 access?

A) AWS IAM  
B) AWS IAM Identity Center  
C) Amazon Cognito  
D) AWS Directory Service

**Answer: C** — Cognito User Pool handles user authentication (email, Google). Cognito Identity Pool provides temporary AWS credentials for S3 access. IAM is for AWS team members. IAM Identity Center is for employee SSO. Directory Service is for Microsoft AD.
