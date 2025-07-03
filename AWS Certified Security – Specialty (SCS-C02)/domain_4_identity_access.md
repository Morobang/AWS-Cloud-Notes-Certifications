# 🔐 Domain 4: Identity and Access Management (AWS SCS-C02)

## ✅ Task 4.1: Authentication for AWS Resources

### 🧠 What This Means
Authentication is about **proving who you are** before you access AWS resources. It’s the first step in access control.

### 🔍 Key Concepts
- **Identity Sources**:
  - **Federation**: Use external identity providers (Google, Microsoft, etc.)
  - **IAM Identity Center (AWS SSO)**: Manage workforce identities across accounts
  - **Amazon Cognito**: Manage app users (e.g., sign-up/sign-in for web/mobile apps)
- **Credential Types**:
  - **Long-term**: Access keys for IAM users (not recommended for production)
  - **Temporary**: Short-lived tokens via AWS STS (recommended and safer)

### 🛠️ Key Skills
- Set up authentication using the right identity system
- Enable **Multi-Factor Authentication (MFA)** for secure logins
- Use **AWS STS** to create temporary credentials for short-lived sessions
- Troubleshoot login issues using:
  - **CloudTrail** (shows login events)
  - **IAM Access Advisor** (see what services users accessed)
  - **IAM Policy Simulator** (test if a user should be able to do something)

---

## ✅ Task 4.2: Authorization for AWS Resources

### 🧠 What This Means
Authorization determines **what actions a user or role is allowed to perform** after they’ve authenticated.

### 🔍 Key Concepts
- **Types of IAM Policies**:
  - **Managed Policies**: Reusable and AWS-managed
  - **Inline Policies**: Attached directly to one user/role
  - **Identity-based**: Attached to users, groups, or roles
  - **Resource-based**: Attached to the resource itself (e.g., S3 bucket policy)
  - **Session Policies**: Applied during session creation for additional restrictions
- **Policy Components**:
  - **Principal**: Who
  - **Action**: What
  - **Resource**: On what
  - **Condition**: When or under what conditions

### 🛠️ Key Skills
- Build **ABAC** (Attribute-Based Access Control): Access based on tags or attributes
- Use **RBAC** (Role-Based Access Control): Assign roles based on job functions
- Pick the right policy type for the job
- Apply **least privilege**: Give only what’s needed, nothing more
- Separate duties (e.g., one person deploys, another approves)
- Investigate authorization issues:
  - Use **IAM Policy Simulator** to test permissions
  - Use **CloudTrail** to trace denied actions
  - Spot unintended permissions and fix them

---

## 📌 Summary Table

| Task | What You Do | AWS Tools |
|------|-------------|------------|
| 4.1 | Authentication | IAM, Cognito, IAM Identity Center, STS, CloudTrail |
| 4.2 | Authorization | IAM Policies, Policy Simulator, Access Advisor, CloudTrail |

---

Let me know if you want flashcards, visual diagrams, or practice questions for this domain!

