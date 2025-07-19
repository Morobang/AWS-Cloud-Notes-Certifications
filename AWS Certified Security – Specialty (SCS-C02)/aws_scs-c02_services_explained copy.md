# **AWS Security Specialty (SCS-C02) – In-Depth Service Breakdown**  
*(For Exam Preparation)*  

---

## **🔐 IAM (Identity & Access Management)**  
### **What it does:**  
Controls **who can access AWS resources** and **what they can do**.  

### **Key Features:**  
✅ **Users & Groups** – Create logins for people/teams (e.g., "Dev-Team").  
✅ **Roles** – Temporary permissions (e.g., "Let this Lambda function access S3").  
✅ **Policies** – JSON rules defining permissions (e.g., "Allow read-only S3 access").  
✅ **MFA (Multi-Factor Auth)** – Extra login security (e.g., phone app + password).  
✅ **Access Analyzer** – Finds unintended resource exposure.  

### **Exam Scenarios:**  
- **Least Privilege Principle** – Give only the **minimum required permissions**.  
- **Role Chaining** – Assume roles across accounts (e.g., Dev → Prod).  
- **Permissions Boundaries** – Limit max permissions a user/role can have.  

### **Pro Tip:**  
- **IAM Policy Simulator** → Test policies before applying them.  

---

## **📜 AWS Organizations**  
### **What it does:**  
Manages **multiple AWS accounts** under one **master account**.  

### **Key Features:**  
✅ **SCPs (Service Control Policies)** – Block risky actions (e.g., "No EC2 in us-east-1").  
✅ **Consolidated Billing** – Single bill for all accounts.  
✅ **Delegated Admin** – Let one account manage services (e.g., Security Hub).  

### **Exam Scenarios:**  
- **Prevent accidental deletions** (e.g., block `s3:DeleteBucket`).  
- **Enforce region restrictions** (e.g., "Only us-west-2 allowed").  
- **Isolate workloads** (e.g., Prod vs. Dev accounts).  

### **Pro Tip:**  
- **AWS Control Tower** = Pre-built **multi-account setup** with guardrails.  

---

## **🔍 AWS CloudTrail**  
### **What it does:**  
Logs **every API call** in your AWS account (who did what, when, from where).  

### **Key Features:**  
✅ **Management & Data Events** – Logs **account changes** (e.g., IAM updates) + **data operations** (e.g., S3 object access).  
✅ **Trail Logs** – Stored in **S3** (encrypted by default).  
✅ **CloudTrail Insights** – Detects unusual activity (e.g., sudden spike in API calls).  

### **Exam Scenarios:**  
- **Forensic investigation** – "Who deleted this S3 bucket?"  
- **Compliance audits** – "Prove no one modified security groups last month."  
- **Detect brute-force attacks** – "100 failed IAM login attempts in 5 mins."  

### **Pro Tip:**  
- **Enable in ALL regions** (attacks often come from unexpected regions).  

---

## **AWS GuardDuty**  
### **What it does:**  
**AI-powered threat detection** (malware, Bitcoin mining, compromised credentials).  

### **Key Features:**  
✅ **Analyzes CloudTrail, VPC Flow Logs, DNS logs**.  
✅ **Detects:**  
   - Unusual API calls (e.g., "IAM user logging in from Nigeria").  
   - Crypto-mining (e.g., "EC2 instance using 100% CPU").  
   - Data exfiltration (e.g., "Massive S3 download to a hacker IP").  

### **Exam Scenarios:**  
- **Responding to findings** – Automate blocking with Lambda + SNS.  
- **Multi-account setup** – Send findings to a **central Security account**.  

### **Pro Tip:**  
- **Suppression Rules** – Ignore **false positives** (e.g., pentesting IPs).  

---

## **Amazon Macie**  
### **What it does:**  
**Finds sensitive data** (PII, credit cards) in **S3 buckets**.  

### **Key Features:**  
✅ **Machine Learning-based discovery** – Scans files for **SSNs, credit cards, etc.**  
✅ **Alerts on exposed data** – "This bucket has public access + 100 credit card numbers!"  
✅ **Integrates with Security Hub** – Centralized alerts.  

### **Exam Scenarios:**  
- **Compliance reports** – "Prove no customer PII is exposed."  
- **Automated remediation** – Trigger Lambda to **encrypt or quarantine** files.  

### **Pro Tip:**  
- **Use before migrating data** – Avoid accidentally uploading sensitive files.  

---

## **AWS Config**  
### **What it does:**  
**Tracks resource changes** & checks **compliance rules**.  

### **Key Features:**  
✅ **Configuration History** – "What did this EC2 instance look like yesterday?"  
✅ **Config Rules** – Auto-check policies (e.g., "All EBS volumes must be encrypted").  
✅ **Remediation** – Auto-fix issues (e.g., enable S3 encryption).  

### **Exam Scenarios:**  
- **Drift Detection** – "Someone manually disabled encryption—alert!"  
- **PCI DSS Compliance** – "Ensure all resources meet PCI rules."  

### **Pro Tip:**  
- **Aggregate data across accounts** → Use **Config Aggregator**.  

---

## **AWS Security Hub**  
### **What it does:**  
**Central dashboard** for security findings (GuardDuty, Macie, Inspector, etc.).  

### **Key Features:**  
✅ **Security Standards** – Pre-built checks for **CIS, PCI DSS, etc.**  
✅ **Automated Response** – Trigger Lambda when high-severity findings appear.  

### **Exam Scenarios:**  
- **Prioritize threats** – Critical vs. low-risk alerts.  
- **Cross-account visibility** – View all findings in one place.  

### **Pro Tip:**  
- **Enable AWS Foundational Security Best Practices** framework.  

---

## **AWS WAF & Shield**  
### **What it does:**  
- **WAF** → Blocks **web attacks** (SQLi, XSS).  
- **Shield** → Stops **DDoS attacks**.  

### **Exam Scenarios:**  
- **WAF Rules:**  
  - Block IPs from known bad countries.  
  - Rate-limit login pages to stop brute force.  
- **Shield Advanced:**  
  - Protect **Elastic IPs, CloudFront, ALB**.  

### **Pro Tip:**  
- **Use AWS Managed Rules** (e.g., "OWASP Top 10").  

---

## **Summary Cheat Sheet**  
| Service          | Best For…                          | Key Exam Concept                     |  
|------------------|------------------------------------|--------------------------------------|  
| **IAM**          | Controlling access                 | Least Privilege, Roles > Users       |  
| **Organizations**| Multi-account governance           | SCPs, Delegated Admin                |  
| **CloudTrail**   | Auditing API calls                 | Enable in ALL regions                |  
| **GuardDuty**    | Threat detection                   | CloudTrail + VPC Flow Log analysis   |  
| **Macie**        | Finding sensitive data             | S3 PII detection                     |  
| **Config**       | Compliance tracking                | Drift detection, Auto-Remediation    |  
| **Security Hub** | Centralized security dashboard     | Aggregates findings from other tools |  
| **WAF/Shield**   | Web & DDoS protection              | Rate-limiting, OWASP rules           |  

---

### **Next Steps for Exam Prep:**  
1. **Hands-On Practice:**  
   - Set up **GuardDuty + Security Hub** in a test account.  
   - Create an **SCP to block root account actions**.  
2. **Mock Exams:**  
   - Focus on **scenario-based questions** (e.g., "How to investigate a data leak?").  
3. **Deep Dives:**  
   - **KMS (Key Management)** – Customer-managed vs. AWS-managed keys.  
   - **VPC Security** – NACLs vs. Security Groups.  















---

### **AWS Audit Manager**  
**Think of it like an automated accountant for security compliance.**  
Instead of manually collecting proof that your AWS environment follows rules (like HIPAA or PCI DSS), Audit Manager does it for you. It continuously takes "snapshots" of your settings—like checking if all S3 buckets are encrypted or if CloudTrail logging is turned on—and organizes them into pre-built reports.  

**Example for the exam:**  
- If an auditor asks, *"Prove that all databases are encrypted,"* Audit Manager automatically gathers evidence (e.g., screenshots of KMS encryption settings for RDS) and generates a report.  

**Key point:** It saves time during audits by auto-collecting evidence from AWS Config, CloudTrail, and other services.  

---

### **AWS Certificate Manager (ACM)**  
**Imagine ACM as a free, auto-renewing SSL certificate vending machine.**  
It lets you easily get SSL/TLS certificates to encrypt traffic to your websites (turning `http://` into `https://`). The certificates work seamlessly with AWS services like CloudFront (CDN) and ALB (load balancers), and they renew automatically so you never face expiry-related outages.  

**Example for the exam:**  
- You deploy a website on EC2 with an ALB. Instead of buying a certificate from GoDaddy, you use **ACM to get a free cert** and attach it to the ALB in 2 clicks.  

**Key point:** ACM only works with AWS services—you can’t export certs for on-prem servers.  

---

### **AWS CloudHSM**  
**This is like a "bank vault" for encryption keys.**  
While AWS KMS lets AWS manage encryption keys for you (with some customer control), CloudHSM gives you **dedicated hardware devices (HSMs)** where you fully control the keys. Even AWS can’t access them, which is critical for strict compliance (e.g., financial or government data).  

**Example for the exam:**  
- A bank wants to encrypt customer data but **must** hold the keys themselves for regulatory reasons. They use CloudHSM to generate and store keys, then use those keys to encrypt DynamoDB or RDS.  

**Key point:** CloudHSM is for when KMS isn’t compliant enough (e.g., FIPS 140-2 Level 3 requirements).  

---

### **Amazon Detective**  
**Think of it as a "security camera replay" for AWS.**  
When GuardDuty detects a hack (e.g., stolen IAM credentials), Detective helps you investigate **how it happened**. It analyzes CloudTrail logs, VPC traffic, and other data to visually map out the attacker’s steps—like which resources they accessed or how they moved laterally.  

**Example for the exam:**  
- GuardDuty alerts you: *"An IAM user from your company is logging in from Russia."* You open Detective to see:  
  1. The attacker first exploited a public EC2 instance.  
  2. They stole IAM keys from the instance metadata.  
  3. Then they downloaded sensitive S3 files.  

**Key point:** Detective doesn’t prevent attacks—it helps you understand and respond to them.  

---

### **AWS Directory Service**  
**This is "Microsoft Active Directory (AD), but hosted in AWS."**  
It lets your employees log in to AWS (and other apps) using their **existing corporate usernames/passwords**, eliminating the need to manage separate AWS IAM users.  

**Two main flavors:**  
1. **AWS Managed Microsoft AD**: A full, standalone AD hosted by AWS (for cloud-only companies).  
2. **AD Connector**: A bridge to your on-premises AD (for hybrid setups).  

**Example for the exam:**  
- Your company uses on-prem AD. You deploy **AD Connector** so employees can log in to AWS with the same credentials they use for their laptops.  

**Key point:** Use **Simple AD** only for test environments—it lacks advanced AD features.  

---

### **AWS Firewall Manager**  
**This is a "central command center" for firewall rules across multiple AWS accounts.**  
Instead of manually configuring WAF (Web Application Firewall) or Shield in every account, Firewall Manager lets you **define rules once** and automatically apply them everywhere (even to new accounts).  

**Example for the exam:**  
- Your company has 50 AWS accounts. You use Firewall Manager to enforce:  
  - A WAF rule blocking SQL injection attacks **on all ALBs**.  
  - Shield Advanced protection **on all CloudFront distributions**.  

**Key point:** Requires **AWS Organizations** and must be enabled in a **central security account**.  

---

### **AWS IAM Identity Center (formerly SSO)**  
**This is "single sign-on (SSO) for AWS."**  
It lets employees log in **once** with their corporate email/password (e.g., via Microsoft AD or Okta) and access **multiple AWS accounts** without needing separate IAM users.  

**Example for the exam:**  
- A contractor needs access to **Dev, Test, and Prod accounts**. Instead of creating 3 IAM users, you:  
  1. Add them to IAM Identity Center.  
  2. Assign permission sets (e.g., "Read-Only" in Prod, "Admin" in Dev).  

**Key point:** Permission sets are like reusable IAM roles for SSO users.  

---

### **Amazon Inspector**  
**This is a "vulnerability scanner" for EC2 instances and containers.**  
It automatically checks for:  
- **Software vulnerabilities** (e.g., outdated Apache versions with known CVEs).  
- **Network exposure** (e.g., accidentally open SSH ports to the internet).  

**Example for the exam:**  
- You run an Inspector scan and it finds:  
  - An EC2 instance running an unpatched version of OpenSSL (CVE-2023-1234).  
  - A container image with a critical vulnerability.  

**Key point:** Inspector is **agent-based**—you must install the Inspector agent on EC2 instances.  

---

### **AWS Network Firewall**  
**This is a "next-gen firewall" for your VPC (like Palo Alto or Fortinet, but AWS-native).**  
It inspects and filters traffic at the network level (Layer 3/4) using **Suricata rules** (open-source IDS/IPS).  

**Example for the exam:**  
- You want to block all outbound traffic to known malware domains. You:  
  1. Deploy Network Firewall in your VPC.  
  2. Add a Suricata rule blocking `*.malware.com`.  

**Key point:** Unlike Security Groups (which are simple allow/deny rules), Network Firewall supports **deep packet inspection**.  

---

### **AWS Shield**  
**This is "DDoS protection" for AWS resources.**  
- **Shield Standard**: Free, auto-protects all AWS customers (basic DDoS mitigation).  
- **Shield Advanced**: Paid, with 24/7 support, cost protection (for scaling during attacks), and custom mitigations.  

**Example for the exam:**  
- Your e-commerce site gets hit by a massive DDoS attack. Shield Advanced:  
  1. Automatically absorbs the attack.  
  2. Credits your bill for scaling costs (e.g., extra ALB capacity).  

**Key point:** Shield Advanced is needed for **custom protections** (e.g., safeguarding Elastic IPs).  

---

### **Summary: "What’s the Key Security Use Case?"**  
| Service                | One-Sentence Security Purpose                          |  
|------------------------|-------------------------------------------------------|  
| **Audit Manager**      | Automates compliance evidence collection for audits.   |  
| **ACM**               | Provides free SSL certs for AWS services.              |  
| **CloudHSM**          | Stores encryption keys in FIPS-compliant hardware.     |  
| **Detective**         | Investigates how a hacker breached your environment.   |  
| **Directory Service** | Lets employees log in to AWS with corporate credentials. |  
| **Firewall Manager**  | Deploys WAF/Shield rules across all AWS accounts.      |  
| **IAM Identity Center**| Single sign-on (SSO) for AWS and business apps.        |  
| **Inspector**         | Scans EC2/containers for vulnerabilities.              |  
| **Network Firewall**  | Filters malicious traffic at the VPC level.             |  
| **Shield**            | Stops DDoS attacks on AWS resources.                   |  

---

### **Exam Pro Tips:**  
1. **GuardDuty + Detective** are often paired—Detective helps investigate GuardDuty findings.  
2. **Firewall Manager requires AWS Organizations** (multi-account setup).  
3. **Inspector vs. Macie**:  
   - Inspector → Finds vulnerabilities (e.g., software flaws).  
   - Macie → Finds sensitive data (e.g., PII in S3).  
4. **Shield Advanced is only for "always-on" DDoS protection** (e.g., critical apps).  












---

# **Complete AWS Security Specialty (SCS-C02) Service Explanations**  

## **Management & Governance Services**  

### **1. AWS CloudTrail**  
**What it does:** Records every API call made in your AWS account (who did what, when, and from where).  
**Example:**  
- If someone deletes an S3 bucket, CloudTrail logs:  
  - **Who** deleted it (IAM user/role)  
  - **When** it happened  
  - **From which IP**  
**Exam Tip:** Enable **multi-region logging** and **integrity validation** to detect tampering.  

---

### **2. Amazon CloudWatch**  
**What it does:** Monitors AWS resources (CPU, memory, logs) and triggers alarms.  
**Security Use Case:**  
- Set an alarm if **"FailedLoginAttempts"** exceed 5 in 5 minutes (brute-force attack detection).  
**Exam Tip:** CloudWatch Logs Insights can query logs (e.g., find all `DeleteNetworkAcl` events).  

---

### **3. AWS Config**  
**What it does:** Tracks changes to AWS resources and checks compliance rules.  
**Example:**  
- Rule: *"All S3 buckets must be encrypted."*  
- If someone disables encryption, AWS Config flags it as **"Non-compliant."**  
**Exam Tip:** Use **Config Aggregator** to view compliance across multiple accounts.  

---

### **4. AWS Organizations**  
**What it does:** Manages multiple AWS accounts under one "parent account."  
**Key Features:**  
- **SCPs (Service Control Policies):** Block risky actions (e.g., "No EC2 in us-east-1").  
- **Consolidated Billing:** One bill for all accounts.  
**Exam Scenario:**  
- *"How to prevent dev teams from creating IAM users?"* → Use an SCP to block `iam:CreateUser`.  

---

### **5. AWS Systems Manager (SSM)**  
**What it does:** Securely manages servers (patch, run commands, troubleshoot).  
**Security Use Cases:**  
- **Session Manager:** Connect to EC2 **without SSH ports open** (reduces attack surface).  
- **Patch Manager:** Automatically installs security updates.  
**Exam Tip:** SSM **requires** the SSM Agent installed on EC2 instances.  

---

### **6. AWS Trusted Advisor**  
**What it does:** Scans your AWS account for security/cost optimization opportunities.  
**Key Checks:**  
- **Security:** Open S3 buckets, unused IAM keys.  
- **Cost:** Idle EC2 instances, unattached EBS volumes.  
**Exam Tip:** **Only the Business/Enterprise Support plan** provides **all** checks.  

---

## **Networking & Content Delivery**  

### **7. Amazon VPC (Virtual Private Cloud)**  
**What it does:** Isolated virtual network for AWS resources.  
**Key Security Features:**  
- **Security Groups:** Firewall for EC2 instances (stateful—allow/deny traffic).  
- **NACLs (Network ACLs):** Subnet-level firewall (stateless—no memory of connections).  
- **VPC Endpoints:** Private connections to AWS services (no internet needed).  
**Exam Scenario:**  
- *"How to block all inbound traffic from 192.0.2.0/24?"* → Use a **NACL** (stateless).  

---

## **Security, Identity & Compliance**  

### **8. AWS Audit Manager**  
**What it does:** Automates compliance evidence collection (HIPAA, PCI DSS).  
**Example:**  
- Auto-generates reports proving:  
  - "All EBS volumes are encrypted."  
  - "MFA is enabled for root users."  
**Exam Tip:** Integrates with **AWS Config** and **CloudTrail**.  

---

### **9. AWS Certificate Manager (ACM)**  
**What it does:** Provides free SSL/TLS certificates for AWS services.  
**Key Limitation:** Only works with **AWS services** (ALB, CloudFront, API Gateway).  
**Exam Scenario:**  
- *"How to enable HTTPS for a CloudFront distribution?"* → Use ACM to provision a cert.  

---

### **10. AWS CloudHSM**  
**What it does:** Dedicated hardware (HSMs) for managing encryption keys.  
**Why use it over KMS?**  
- **You fully control keys** (AWS can’t access them).  
- **FIPS 140-2 Level 3 compliance** (for strict regulations).  
**Exam Scenario:**  
- *"A bank needs to store encryption keys for regulatory compliance."* → Use CloudHSM.  

---

### **11. Amazon Detective**  
**What it does:** Investigates security incidents by analyzing logs.  
**Example:**  
- GuardDuty detects a compromised IAM user.  
- Detective shows:  
  - Which API calls the attacker made.  
  - Which resources were accessed.  
**Exam Tip:** Does **not prevent attacks**—only helps investigate.  

---

### **12. AWS Directory Service**  
**What it does:** Integrates AWS with Microsoft Active Directory (AD).  
**Options:**  
- **AWS Managed AD:** Fully managed Microsoft AD in AWS.  
- **AD Connector:** Proxy for on-premises AD.  
**Exam Scenario:**  
- *"Employees need to log in to AWS with their corporate credentials."* → Use AD Connector.  

---

### **13. AWS Firewall Manager**  
**What it does:** Centrally manages WAF & Shield rules across accounts.  
**Requirements:**  
- Must use **AWS Organizations**.  
- Must enable in a **central security account**.  
**Exam Scenario:**  
- *"How to enforce WAF rules on all ALBs in 50 accounts?"* → Use Firewall Manager.  

---

### **14. Amazon GuardDuty**  
**What it does:** AI-powered threat detection (malware, Bitcoin mining, compromised credentials).  
**Data Sources:**  
- CloudTrail (API calls)  
- VPC Flow Logs (network traffic)  
- DNS logs  
**Exam Scenario:**  
- *"An IAM user suddenly starts deleting S3 buckets from Nigeria."* → GuardDuty flags it.  

---

### **15. AWS IAM Identity Center (SSO)**  
**What it does:** Single sign-on (SSO) for AWS accounts & business apps.  
**Key Feature:** **Permission sets** (reusable IAM roles for SSO users).  
**Exam Scenario:**  
- *"Contractors need temporary access to Dev/Test accounts."* → Assign time-bound permission sets.  

---

### **16. AWS IAM (Identity & Access Management)**  
**What it does:** Controls who can access AWS resources.  
**Key Concepts:**  
- **Users/Groups:** People (e.g., "developers").  
- **Roles:** Temporary permissions (e.g., for Lambda functions).  
- **Policies:** JSON rules defining permissions.  
**Exam Tip:** **Least Privilege Principle** = Give only the minimum required permissions.  

---

### **17. Amazon Inspector**  
**What it does:** Scans EC2 instances & containers for vulnerabilities.  
**Checks:**  
- **Network reachability** (open ports).  
- **Software CVEs** (outdated libraries).  
**Exam Scenario:**  
- *"How to find unpatched OpenSSL vulnerabilities?"* → Run an Inspector scan.  

---

### **18. AWS KMS (Key Management Service)**  
**What it does:** Manages encryption keys for AWS services.  
**Key Types:**  
- **AWS-managed keys** (default, free).  
- **Customer-managed keys (CMK)** (you control rotation & policies).  
**Exam Scenario:**  
- *"How to ensure only the finance team can decrypt S3 files?"* → Use a CMK with a key policy.  

---

### **19. Amazon Macie**  
**What it does:** Uses AI to find sensitive data (PII, credit cards) in S3.  
**Example:**  
- Scans S3 buckets and flags: *"Bucket X has 50 unencrypted credit card numbers."*  
**Exam Tip:** Macie **only works with S3** (not RDS, DynamoDB, etc.).  

---

### **20. AWS Network Firewall**  
**What it does:** Stateful firewall for VPC traffic (Layer 3/4).  
**Uses Suricata rules** (like an IDS/IPS).  
**Exam Scenario:**  
- *"How to block outbound traffic to known malware IPs?"* → Deploy Network Firewall with a Suricata rule.  

---

### **21. AWS Security Hub**  
**What it does:** Central dashboard for security findings (GuardDuty, Macie, Inspector).  
**Key Feature:** **AWS Foundational Security Best Practices** (pre-built compliance checks).  
**Exam Scenario:**  
- *"How to track security posture across 20 accounts?"* → Enable Security Hub in all accounts.  

---

### **22. AWS Shield**  
**What it does:** Protects against DDoS attacks.  
- **Shield Standard:** Free (basic protection).  
- **Shield Advanced:** Paid (24/7 support, cost protection).  
**Exam Scenario:**  
- *"How to protect a gaming website from DDoS?"* → Use Shield Advanced + WAF.  

---

### **23. AWS WAF (Web Application Firewall)**  
**What it does:** Blocks web attacks (SQLi, XSS) at Layer 7.  
**Key Features:**  
- **Rate-based rules** (block brute-force attacks).  
- **AWS Managed Rules** (pre-configured for OWASP Top 10).  
**Exam Scenario:**  
- *"How to block SQL injection attacks?"* → Deploy WAF with the **AWS Managed SQLi rule**.  

---

## **Final Exam Cheat Sheet**  
| Service | Key Security Use Case | Must-Know Fact |  
|---------|----------------------|----------------|  
| **CloudTrail** | Logs all API calls | Enable multi-region logging |  
| **GuardDuty** | Threat detection | Uses CloudTrail + VPC Flow Logs |  
| **IAM** | Access control | Apply least privilege |  
| **KMS** | Encryption keys | Customer-managed keys (CMKs) allow key rotation |  
| **Macie** | Finds PII in S3 | Only works with S3 |  
| **WAF** | Blocks SQLi/XSS | Use rate-limiting for login pages |  

---

### **Next Steps for Exam Prep:**  
1. **Hands-On Practice:**  
   - Enable **GuardDuty** in your AWS account.  
   - Create an **SCP** to block root account actions.  
2. **Mock Exams:** Focus on:  
   - **SCPs vs. IAM Policies** (SCPs are account-wide, IAM is per-user).  
   - **GuardDuty vs. Inspector** (threat detection vs. vulnerability scanning).  
3. **White Papers:** Read:  
   - [AWS Security Pillar (Well-Architected Framework)](https://aws.amazon.com/architecture/well-architected/)  
   - [AWS Encryption Best Practices](https://docs.aws.amazon.com/whitepapers/latest/kms-best-practices/introduction.html)  

