# 🏛️ Domain 6: Management and Security Governance (AWS SCS-C02)

## ✅ Task 6.1: Central Account Management Strategy

### 🧠 What This Means
This is about how to set up and manage **multiple AWS accounts in an organized, secure way**.

### 🔍 Key Concepts
- **Multi-account strategies**: Separate accounts by function (e.g., dev, prod, security)
- **Delegated Admin**: Allow specific accounts (e.g., security) to manage certain services across all accounts
- **Guardrails**: Rules to control actions across accounts using **Service Control Policies (SCPs)**
- **Root Account Best Practices**:
  - Use only for emergencies
  - Enable MFA, don’t use root access keys
- **Cross-account Roles**: Allow one account to access another securely

### 🛠️ Key Skills
- Set up and configure **AWS Organizations**
- Use **AWS Control Tower** for easy multi-account setup
- Deploy SCPs to limit risky actions (e.g., block use of root)
- Aggregate findings centrally (e.g., **Security Hub**, **AWS Config Aggregator**)
- Secure the root account (MFA, remove access keys, auditing)

---

## ✅ Task 6.2: Secure and Consistent Deployment Strategy

### 🧠 What This Means
Use tools and practices to **safely and consistently deploy cloud infrastructure** across teams and environments.

### 🔍 Key Concepts
- **Infrastructure as Code (IaC)**:
  - Use CloudFormation templates
  - Check for drift (unexpected changes)
- **Tagging**:
  - Use standardized tags to track cost, ownership, environment
- **Centralized Deployment**:
  - Use **Service Catalog** to create portfolios of approved services
- **Visibility & Control**:
  - Know what’s deployed, where, and by whom

### 🛠️ Key Skills
- Use **CloudFormation** to deploy secure and consistent environments
- Enforce tagging policies (for reporting, billing, automation)
- Deploy portfolios via **AWS Service Catalog**
- Use **AWS Firewall Manager** to apply consistent security rules
- Share resources securely using **AWS RAM** (e.g., subnets, databases, etc.)

---

## ✅ Task 6.3: Evaluate Compliance of AWS Resources

### 🧠 What This Means
Make sure your AWS infrastructure follows company or legal **compliance standards**.

### 🔍 Key Concepts
- **Data Classification**:
  - Use **Amazon Macie** to find and classify sensitive data (e.g., PII)
- **Resource Auditing**:
  - Use **AWS Config** to monitor compliance with rules (e.g., encryption enabled)
- **Evidence Collection**:
  - Use **Security Hub** and **AWS Audit Manager** for audits and reports

### 🛠️ Key Skills
- Identify sensitive data with Macie
- Write and apply **AWS Config Rules** to detect misconfigurations
- Gather compliance evidence via Audit Manager and Security Hub

---

## ✅ Task 6.4: Identify Security Gaps and Cost Issues

### 🧠 What This Means
Regularly check your infrastructure for **security weaknesses and wasted costs**.

### 🔍 Key Concepts
- **Cost + Usage Review**:
  - Use **Cost Explorer** and **Trusted Advisor** to find unused resources or overuse
- **Attack Surface Minimization**:
  - Remove unused services, unneeded ports, or unnecessary permissions
- **AWS Well-Architected Framework**:
  - A structured guide for checking security, reliability, cost, and performance

### 🛠️ Key Skills
- Detect anomalies in usage and spending trends
- Find and remove unused resources
- Use the **Well-Architected Tool** to review and improve security posture

---

## 📌 Summary Table

| Task | What You Do | Key AWS Tools |
|------|-------------|----------------|
| 6.1 | Manage Accounts | AWS Organizations, Control Tower, SCPs, IAM, Config Aggregator |
| 6.2 | Secure Deployments | CloudFormation, Service Catalog, Firewall Manager, RAM |
| 6.3 | Evaluate Compliance | Macie, AWS Config, Security Hub, Audit Manager |
| 6.4 | Find Gaps & Costs | Trusted Advisor, Cost Explorer, Well-Architected Tool |

---

Let me know if you want a **practice quiz**, **PDF compilation**, or diagrams for this domain!

