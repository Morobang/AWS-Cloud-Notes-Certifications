# Task Statement 2.3: Identify AWS Access Management Capabilities

## What you'll learn
- Authentication (proving who you are) vs. authorization (what you can do)
- The IAM building blocks: users, groups, roles, and policies
- The root user — when to use it and when not to
- Multi-factor authentication (MFA) and why it matters
- How roles enable services to access other services
- The principle of least privilege

---

## Authentication vs. authorization

These two words are often confused. They describe different parts of the access control process.

**Authentication** is proving you are who you claim to be. When you type a username and password, you're authenticating — presenting evidence of your identity.

**Authorization** is determining what you're allowed to do once your identity is confirmed. After AWS knows who you are, it checks your permissions to decide whether your request is allowed.

The distinction matters on the exam: CloudTrail records who made a request (authentication context); IAM policies determine whether they're allowed (authorization).

---

## IAM — Identity and Access Management

IAM is the AWS service that controls who can access which AWS resources and what they can do with them. Everything in this task flows through IAM.

IAM operates at four levels: users, groups, roles, and policies. Understanding what each is and how they relate is the core of this task statement.

### Users

An IAM user is an identity that represents a person or application. Each user has:
- A name and unique ID
- Credentials (a password for console access, or access keys for programmatic access)
- Permissions (attached directly or inherited from groups)

Users are appropriate for individual people who need long-term access to your AWS account — a developer who needs to log into the console, or an application that needs access keys to call the API.

One person = one user. Never share user credentials between people. If a shared account takes a bad action, you can't tell who did it.

### Groups

A group is a collection of users. You attach permissions to the group, and all users in the group inherit those permissions.

Instead of assigning permissions to Sarah, James, and Wei individually, you create a "Developers" group, attach the right permissions to the group, and add all three as members. When a fourth developer joins, you add them to the group — they instantly have the right permissions. When Sarah moves to a different team, you remove her from the group — permissions gone.

Groups simplify permission management dramatically at scale.

One important note: groups cannot contain other groups. A user can belong to multiple groups; a group cannot be a member of another group.

### Roles

A role is an identity that can be temporarily assumed by a user, an application, or an AWS service. Unlike users, roles don't have long-term credentials — they issue temporary credentials that expire automatically.

**The critical use case: giving AWS services permissions to access other AWS services.**

If an EC2 instance needs to read files from S3, the correct approach is to create an IAM role with S3 read permissions and attach it to the EC2 instance. The instance temporarily assumes that role. No credentials need to be stored on the server.

This is better than creating an IAM user and storing access keys on the instance because:
- Temporary credentials automatically expire and rotate
- No secret value is sitting in a config file that could be leaked
- If the instance is compromised, the credentials expire quickly

Other role use cases:
- **Cross-account access**: A user in Account A assumes a role in Account B to access its resources
- **Federated access**: Corporate employees use their company login to assume an AWS role, without creating individual AWS accounts
- **AWS service roles**: Services like Lambda, ECS, and CodePipeline need roles to act on your behalf

### Policies

A policy is a JSON document that defines what actions are allowed or denied on which resources. Policies are attached to users, groups, or roles.

A policy has three main components:
- **Effect**: Allow or Deny
- **Action**: Which API actions are covered (e.g., `s3:GetObject`, `ec2:StartInstances`)
- **Resource**: Which specific resources the rule applies to (a specific S3 bucket, all EC2 instances, etc.)

**AWS-managed policies** are pre-built policies maintained by AWS. `AmazonS3ReadOnlyAccess`, `AdministratorAccess`, `PowerUserAccess` are examples. They're convenient but broad.

**Customer-managed policies** are policies you write. They can be more precise — exactly the permissions a specific role needs, nothing more.

**Inline policies** are policies embedded directly in a single user, group, or role rather than existing as a standalone object. Useful for one-off permissions, but harder to manage at scale.

The order of evaluation: if any policy has an explicit Deny, that Deny wins regardless of other Allow statements. If there's no explicit Allow, access is denied by default.

---

## The root user

When you first create an AWS account, you create it with an email and password. The identity tied to that email is the **root user**. It has unrestricted access to everything in the account — nothing can block it.

There are a small number of tasks that only the root user can perform:
- Closing the AWS account
- Changing the account's root email address
- Changing the AWS support plan
- Enabling IAM access to billing information
- Accepting certain marketplace agreements

For everything else, you should use IAM users or roles.

**Best practices for the root user:**
1. Enable MFA immediately — this is the most important step
2. Create a strong, unique password stored securely
3. Create an IAM administrator user for day-to-day administration
4. Don't use the root user for routine tasks
5. Don't create access keys for the root user

The reason to avoid the root user for daily work: if the root user's credentials are compromised, the attacker can do anything — change passwords, close the account, delete all resources. With IAM users, you can limit the blast radius of a compromised credential.

---

## Multi-factor authentication (MFA)

Passwords alone are a single point of failure. If someone steals or guesses your password, they have full access. MFA requires a second factor — something you physically have — in addition to the password.

The three types of authentication factors:
- **Something you know** — password, PIN
- **Something you have** — phone, hardware token, authenticator app
- **Something you are** — fingerprint, face ID (biometrics)

MFA combines at least two of these. Even if an attacker obtains your password, they can't log in without the second factor.

**AWS MFA options:**
- **Virtual MFA** (most common) — an authenticator app (Google Authenticator, Authy, etc.) generates a 6-digit time-based code. You enter this code alongside your password.
- **Hardware MFA** — a physical device (like a YubiKey) that generates codes or uses cryptographic keys. More secure than virtual MFA; recommended for the root user and high-privilege accounts.
- **SMS MFA** — a code sent via text message. Less secure than authenticator apps because phone numbers can be hijacked. Not recommended.

For the exam: MFA should always be enabled on the root user. It should also be enabled on any IAM user with administrative access.

---

## Access keys

Access keys are long-term credentials for programmatic access — using the AWS CLI, SDK, or making direct API calls. An access key consists of two parts: an Access Key ID (like a username) and a Secret Access Key (like a password).

Security considerations for access keys:
- Never embed access keys in application code — they can be accidentally committed to source control and exposed
- For applications running on EC2, Lambda, or other AWS services, use roles instead
- If you must use access keys for local development, store them in the AWS credentials file, not in code
- Rotate access keys regularly — the recommended maximum lifetime is 90 days
- Delete access keys that aren't being used

If an access key is compromised, an attacker can call AWS APIs with those credentials indefinitely (until you revoke the key). Temporary credentials from roles are better because they expire automatically — typically after 1–12 hours.

---

## Principle of least privilege

The principle of least privilege means giving every user, role, and service only the minimum permissions needed to do its job — nothing more.

This limits the damage that can be done if credentials are compromised or if a user makes a mistake. A developer with read-only access to a S3 bucket can't accidentally delete it. An application with permission to read from one specific database can't access another.

In practice, it's tempting to give broad permissions (`AdministratorAccess` for every developer) because it's easier. The principle of least privilege says you pay that convenience cost upfront, in exchange for a much smaller security blast radius.

**IAM Access Analyzer** and **AWS Trusted Advisor** can identify permissions that were granted but never used — which suggests they can be safely removed.

---

## IAM best practices summary

| Practice | Why |
|----------|-----|
| Enable MFA on the root user | Root can't be restricted by IAM; compromise is catastrophic |
| Don't use the root user for daily work | Limits blast radius of credential compromise |
| Create individual users, not shared accounts | Maintains audit trail of who did what |
| Use groups to assign permissions | Easier to maintain than per-user assignments |
| Use roles for applications and services | No long-term credentials to manage or leak |
| Apply least privilege | Limits damage from compromised or mistaken actions |
| Rotate access keys regularly | Limits exposure window if keys are stolen |
| Enable IAM Access Analyzer | Identifies overly permissive policies |

---

## Practice questions

**Q1.** A developer needs to write an application running on EC2 that uploads files to S3. What is the most secure way to provide the necessary credentials?

A) Create an IAM user, generate access keys, and store them in the application code  
B) Create an IAM user, generate access keys, and store them in an environment variable on the EC2 instance  
C) Create an IAM role with S3 write permissions and attach it to the EC2 instance  
D) Use the root user's access keys stored in a config file

**Answer: C** — IAM roles provide temporary, automatically rotating credentials to the EC2 instance. No credentials need to be stored anywhere. Options A, B, and D all involve long-term credentials that could be leaked or compromised.

---

**Q2.** A new employee joins the marketing team. Five people already on the team have identical AWS permissions. What is the most efficient way to give the new employee the same permissions?

A) Copy the permissions from one existing user and apply them to the new user  
B) Attach the same policies to the new user one by one  
C) Add the new user to the existing marketing IAM group  
D) Give the new user AdministratorAccess

**Answer: C** — Groups let you manage permissions at scale. Adding the user to the group instantly applies all group permissions. Copying permissions (A, B) creates maintenance burden. AdministratorAccess (D) violates least privilege.

---

**Q3.** A company wants to ensure that only the root user can close their AWS account, while all other tasks are handled by IAM administrators. What enables this behavior?

A) A Service Control Policy that prevents IAM users from closing accounts  
B) The root user inherently has exclusive capability to close the account — IAM users cannot close accounts by design  
C) An IAM policy attached to all users that denies account closure  
D) AWS Organizations management account restrictions

**Answer: B** — Closing the AWS account is a root-user-only action by design. IAM users cannot perform this action regardless of their permissions. It's one of the capabilities reserved exclusively for the root user.

---

**Q4.** A security audit reveals that several IAM users have not logged in for 90+ days, but retain full access to production systems. What security principle is being violated?

A) Separation of duties  
B) Defense in depth  
C) Principle of least privilege  
D) Data sovereignty

**Answer: C** — Least privilege means access should be the minimum necessary. Users who haven't logged in for 90 days likely don't need their access. Retaining it creates unnecessary risk — if their credentials are compromised, an attacker gains full access to production.

---

**Next:** [Task 2.4 — Security Components and Resources](./Task%20Statement%202.4-Identify%20components%20and%20resources%20for%20security.md)
