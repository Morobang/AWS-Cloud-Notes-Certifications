# Domain 2: Security and Compliance — Practice Questions

> 25 questions | ~30% of the CLF-C02 exam (highest weighted domain)

---

**Q1.** Under the AWS Shared Responsibility Model, which of the following is the customer's responsibility?

A) Physical security of AWS data centers  
B) Patching the hypervisor that runs EC2 instances  
C) Managing IAM user accounts and access policies  
D) Maintaining the global network infrastructure

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Customers are responsible for "security IN the cloud" — what they configure and put inside AWS. IAM users, roles, and policies are entirely the customer's responsibility.

A, B, and D are all AWS's responsibility — they fall under "security OF the cloud" (physical infrastructure, hypervisor, global network).
</details>

---

**Q2.** A company is using Amazon RDS for their database. According to the Shared Responsibility Model, what is the customer responsible for?

A) Patching the RDS database engine  
B) Managing the OS on the RDS instance  
C) Managing the data stored in the database and database user permissions  
D) Maintaining the physical hardware running RDS

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

With managed services like RDS, AWS takes over the OS and database engine patching. The customer retains responsibility for the DATA inside the database, the schemas, database user accounts, and who has access to it.

A and B are both AWS's responsibility for RDS (that's the whole point of a managed service).
D is always AWS's responsibility (physical hardware).
</details>

---

**Q3.** Which AWS service should a security team use to get a centralized view of security alerts and compliance status across multiple AWS accounts?

A) Amazon GuardDuty  
B) AWS Security Hub  
C) AWS Config  
D) Amazon Inspector

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

AWS Security Hub is the central security dashboard that aggregates, organizes, and prioritizes security alerts from multiple AWS services and accounts. It shows you compliance status against standards like CIS AWS Foundations.

- A (GuardDuty) detects threats in one account — it feeds INTO Security Hub.
- C (Config) tracks configuration changes, not a security dashboard.
- D (Inspector) scans for vulnerabilities in EC2/containers — also feeds into Security Hub.
</details>

---

**Q4.** A developer accidentally left an access key for a high-privilege IAM user in a public GitHub repository. What is the FIRST thing the security team should do?

A) Enable MFA on the IAM user  
B) Rotate the root user password  
C) Deactivate or delete the exposed access key immediately  
D) Review CloudWatch logs for the past 30 days

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

The immediate priority is to revoke the exposed credential before anyone uses it maliciously. Deactivate or delete the exposed access key right away. Then investigate what was accessed using CloudTrail.

- A (Enable MFA) is a good practice but doesn't stop the exposed key from being used.
- B (Root password) is irrelevant — an IAM user key was exposed, not root credentials.
- D (Review logs) is the second step, not the first — stop the bleeding first.
</details>

---

**Q5.** Which AWS service automatically detects threats such as unauthorized API calls, unusual network traffic, and potential account compromises using machine learning?

A) AWS Config  
B) Amazon Inspector  
C) Amazon GuardDuty  
D) AWS Shield

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Amazon GuardDuty is a threat detection service that continuously monitors CloudTrail logs, VPC Flow Logs, and DNS logs using ML to identify threats like credential exfiltration, crypto mining, or unusual API activity.

- A (Config) tracks resource configuration changes — not threat detection.
- B (Inspector) scans for vulnerabilities — not behavioral threat detection.
- D (Shield) is DDoS protection specifically.
</details>

---

**Q6.** According to AWS best practices, what should be done with the AWS root user account after the initial account setup?

A) Use it for all administrative tasks as it has full permissions  
B) Delete it immediately after creating an IAM admin user  
C) Lock it away — enable MFA and do not use it for everyday tasks  
D) Share its credentials with all members of the IT team

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

AWS best practice: enable MFA on root, store the credentials securely, and use it only for specific tasks that REQUIRE root access (like closing the account, changing the support plan, or enabling consolidated billing). For everything else, use IAM users with appropriate permissions.

- B is wrong — you can't delete the root user.
- A and D are terrible security practices.
</details>

---

**Q7.** A company needs to ensure that developers cannot delete production S3 buckets even if they accidentally try to. Which IAM feature should be used?

A) IAM Password Policy  
B) Permission Boundary  
C) IAM Role  
D) Service Control Policy (SCP)

<details>
<summary>Answer & Explanation</summary>

**Answer: D**

Service Control Policies (SCPs) are applied at the AWS Organizations level and act as guardrails — they limit what actions can be performed in member accounts, even by admins. An SCP denying `s3:DeleteBucket` on production accounts would prevent accidental deletions.

- B (Permission Boundary) limits individual IAM entities but doesn't work across an entire account.
- A (Password Policy) is about login credentials, not permissions.
- C (IAM Role) is an identity, not a restriction mechanism.
</details>

---

**Q8.** Which of the following represents the principle of least privilege?

A) Giving all IAM users the AdministratorAccess policy to ensure they can always get their work done  
B) Granting only the permissions required for a user or service to perform their specific job  
C) Using a single shared IAM user for all team members  
D) Disabling MFA to simplify the login process

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Least privilege = minimum necessary access. If a developer only needs to read from S3 and write to DynamoDB, they should get exactly that and nothing more. If they're compromised, the blast radius is contained.

All other options are security anti-patterns.
</details>

---

**Q9.** What is the difference between an IAM Role and an IAM User?

A) IAM Roles are for humans only; IAM Users are for applications  
B) IAM Roles provide temporary credentials; IAM Users have long-term credentials  
C) IAM Users can only access the AWS console; IAM Roles can only be used programmatically  
D) IAM Roles are more expensive than IAM Users

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

IAM Roles provide temporary security credentials that expire. They're assumed by trusted entities (EC2 instances, Lambda functions, other AWS accounts, federated users). IAM Users have permanent access keys and passwords. Roles are generally more secure because credentials expire automatically.

- A is backwards — IAM Users are usually for humans, Roles are often assumed by services.
</details>

---

**Q10.** A company must demonstrate to auditors that no one has made unauthorized changes to their AWS infrastructure. Which service provides a history of all API calls made in the account?

A) Amazon CloudWatch  
B) AWS CloudTrail  
C) AWS Config  
D) Amazon GuardDuty

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

CloudTrail records every API call made in the AWS account — who made it, from what IP, at what time, and what parameters were used. It's the audit log for compliance purposes.

- A (CloudWatch) shows performance metrics and logs but not API call history.
- C (Config) shows WHAT the configuration was at a point in time but not who made the API call.
- D (GuardDuty) detects threats using those logs, but CloudTrail is the source of the audit trail.
</details>

---

**Q11.** Which AWS service allows a company to access compliance reports such as SOC 2, ISO 27001, and PCI DSS documentation?

A) AWS Trusted Advisor  
B) AWS Security Hub  
C) AWS Artifact  
D) AWS Config

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

AWS Artifact is a self-service portal for on-demand access to AWS compliance reports (SOC, PCI, ISO, etc.) and AWS agreements. When auditors ask for compliance evidence, you pull reports from Artifact.

The others have nothing to do with accessing compliance documentation.
</details>

---

**Q12.** A web application is receiving malicious HTTP requests that attempt SQL injection attacks. Which AWS service should be deployed to filter these requests?

A) AWS Shield  
B) Amazon GuardDuty  
C) AWS WAF  
D) Amazon Macie

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

AWS WAF (Web Application Firewall) inspects incoming HTTP/HTTPS traffic and can block requests based on rules — including SQL injection patterns, XSS, known malicious IPs, and geographic restrictions.

- A (Shield) only protects against DDoS volumetric attacks, not SQL injection.
- B (GuardDuty) detects threats after the fact, doesn't block incoming requests.
- D (Macie) is about finding sensitive data in S3, not web traffic filtering.
</details>

---

**Q13.** Amazon Macie is being evaluated by a healthcare company. What does Macie primarily do?

A) Scans EC2 instances for security vulnerabilities  
B) Detects unusual account activity and threats using machine learning  
C) Uses machine learning to automatically discover and protect sensitive data in Amazon S3  
D) Encrypts data stored in AWS databases

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Amazon Macie uses ML to automatically discover, classify, and protect sensitive data in S3 — things like PII (names, SSNs, credit card numbers), PHI (health information), and credentials. Essential for HIPAA and GDPR compliance.

- A describes Amazon Inspector.
- B describes Amazon GuardDuty.
- D is AWS KMS functionality.
</details>

---

**Q14.** A company wants to ensure all data stored in S3 and RDS is encrypted. Which service manages the encryption keys?

A) AWS Secrets Manager  
B) AWS Certificate Manager  
C) AWS Key Management Service (KMS)  
D) AWS CloudHSM

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

AWS KMS manages cryptographic keys used for encryption. S3, RDS, EBS, and most AWS services integrate with KMS to encrypt data at rest. You control who can use the keys.

- A (Secrets Manager) stores application secrets like passwords and API keys, not encryption keys.
- B (ACM) manages SSL/TLS certificates for HTTPS, not data encryption keys.
- D (CloudHSM) is for dedicated hardware security modules — more control than KMS but more complexity/cost.
</details>

---

**Q15.** AWS Shield Standard is automatically enabled for all AWS customers. What does it protect against?

A) SQL injection and cross-site scripting attacks  
B) Common network and transport layer DDoS attacks  
C) Unauthorized access to S3 buckets  
D) Malware and ransomware in EC2 instances

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

AWS Shield Standard (free, automatic) protects all AWS customers from common DDoS attacks at the network (Layer 3) and transport (Layer 4) layers — like SYN floods and UDP reflection attacks.

Shield Advanced adds protection at the application layer (Layer 7) and includes a dedicated DDoS response team.

- A describes WAF.
- C is handled by S3 bucket policies and IAM.
- D describes security software, not a Shield function.
</details>

---

**Q16.** Which feature of AWS Organizations allows a company to ensure that member accounts can never enable certain AWS services regardless of what IAM policies they set?

A) IAM Permission Boundaries  
B) Service Control Policies (SCPs)  
C) Resource-Based Policies  
D) AWS Config Rules

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

SCPs are organization-level guardrails. Even if an admin in a member account has full IAM permissions, they can't perform actions that are denied by an SCP at the organization or OU level. SCPs set the maximum permissions ceiling.

- A (Permission Boundaries) scope individual IAM entities but don't apply account-wide.
- C (Resource-Based Policies) are attached to specific resources like S3 buckets.
- D (Config Rules) check for compliance but don't enforce or prevent actions.
</details>

---

**Q17.** A company is building a mobile app that requires users to sign in with their Google or Facebook accounts. Which AWS service should handle this identity and authentication?

A) AWS IAM  
B) Amazon Cognito  
C) AWS Single Sign-On  
D) AWS Directory Service

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Amazon Cognito provides identity management for web and mobile apps, including social sign-in (Google, Facebook, Apple), as well as its own user pool for username/password authentication.

- A (IAM) is for AWS users and services, not end-users of your application.
- C (SSO) is for employee access to internal AWS accounts and third-party business apps.
- D (Directory Service) is for managing Windows Active Directory in AWS.
</details>

---

**Q18.** What does "security IN the cloud" refer to in the Shared Responsibility Model?

A) The physical security of AWS data centers  
B) Security of the hypervisor and virtualization layer  
C) Security controls the customer implements within AWS services  
D) AWS's responsibility for maintaining the global network

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

"Security IN the cloud" = customer responsibility. This includes configuring IAM, encrypting data, setting up VPC security groups, patching EC2 OS, and securing application code.

"Security OF the cloud" = AWS responsibility (A, B, D).
</details>

---

**Q19.** Which service tracks whether AWS resources are compliant with company policies and can alert when a configuration drifts from the approved state?

A) AWS CloudTrail  
B) AWS Config  
C) Amazon GuardDuty  
D) AWS Systems Manager

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

AWS Config continuously evaluates resource configurations against defined rules (compliance checks). If an S3 bucket is set to public when your rule says "no public buckets," Config flags it as NON_COMPLIANT and can trigger remediation.

- A (CloudTrail) records API calls but doesn't evaluate compliance.
- C (GuardDuty) detects security threats, not configuration compliance.
- D (Systems Manager) manages EC2 operations (patching, inventory) but isn't a compliance evaluation service.
</details>

---

**Q20.** Which of the following is a benefit of using IAM roles for EC2 instances instead of storing access keys on the instance?

A) IAM Roles are free, while access keys cost money  
B) IAM Roles provide permanent credentials that never change  
C) IAM Roles eliminate the need to store long-term credentials on the instance — credentials are temporary and rotated automatically  
D) IAM Roles give the instance access to all AWS services by default

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

When an IAM role is attached to an EC2 instance, the instance gets temporary credentials via the instance metadata service. These credentials automatically expire and rotate — no one needs to hardcode or manage static access keys. If the instance is compromised, the damage is limited.

- B is wrong — IAM role credentials are TEMPORARY, not permanent.
- D is wrong — roles only provide access to what their policies explicitly allow.
</details>

---

**Q21.** A company stores secrets like API keys and database passwords in AWS. They want the secrets to automatically rotate every 30 days. Which service provides this?

A) AWS KMS  
B) AWS Secrets Manager  
C) AWS Systems Manager Parameter Store  
D) AWS Certificate Manager

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

AWS Secrets Manager stores secrets and can automatically rotate them on a schedule — including native integration with RDS to rotate database passwords without application downtime.

- A (KMS) manages encryption keys, not application secrets.
- C (Parameter Store) can store secrets but does NOT natively rotate them automatically.
- D (ACM) manages SSL/TLS certificates, not application secrets.
</details>

---

**Q22.** Which AWS compliance program is specifically relevant for companies that process credit card transactions?

A) HIPAA  
B) SOC 2  
C) PCI DSS  
D) GDPR

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

PCI DSS (Payment Card Industry Data Security Standard) is the compliance standard for organizations that handle credit card data. AWS has PCI DSS Level 1 certification, and customers can build PCI-compliant workloads on AWS.

- A (HIPAA) = US healthcare data.
- B (SOC 2) = general security and availability controls for service organizations.
- D (GDPR) = EU personal data protection.
</details>

---

**Q23.** What is the purpose of Multi-Factor Authentication (MFA) in AWS?

A) It speeds up the login process by eliminating the need for a password  
B) It provides an additional layer of security by requiring a second form of verification beyond just a password  
C) It automatically encrypts all data stored in S3  
D) It prevents unauthorized API calls from being logged in CloudTrail

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

MFA adds a second factor (like a one-time code from an authenticator app or hardware token) that must be provided along with the password. Even if a password is stolen, the attacker can't log in without the second factor.

MFA is mandatory for root and strongly recommended for all IAM users with console access.
</details>

---

**Q24.** A company wants to scan their EC2 instances and container images for software vulnerabilities and unintended network exposure. Which service should they use?

A) Amazon GuardDuty  
B) Amazon Macie  
C) Amazon Inspector  
D) AWS Security Hub

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Amazon Inspector is an automated vulnerability management service that continuously scans EC2 instances and container images in ECR for software vulnerabilities (CVEs) and unintended network accessibility.

- A (GuardDuty) detects threats from behavioral analysis, not vulnerability scanning.
- B (Macie) finds sensitive data in S3.
- D (Security Hub) aggregates findings from multiple services including Inspector.
</details>

---

**Q25.** Under the Shared Responsibility Model, which of these is an example of AWS being responsible for security?

A) Configuring security groups on EC2 instances  
B) Enabling encryption on S3 buckets  
C) Patching firmware on the physical servers in AWS data centers  
D) Creating IAM users with appropriate permissions

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

AWS is responsible for the physical infrastructure — including the hardware, firmware, and the data center facilities. Customers never have access to patch or manage physical servers.

A, B, and D are all customer responsibilities — they are configuration choices that the customer makes within the AWS environment.
</details>
