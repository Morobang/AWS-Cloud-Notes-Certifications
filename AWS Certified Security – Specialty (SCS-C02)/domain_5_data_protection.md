# 🔒 Domain 5: Data Protection (AWS SCS-C02)

## ✅ Task 5.1: Protect Data in Transit

### 🧠 What This Means
This task is about **encrypting data as it moves** between users, services, and systems — so attackers can't spy or tamper with it.

### 🔍 Key Concepts
- **TLS (Transport Layer Security)**: Encrypts data between client and server
- **VPN (e.g., IPsec)**: Secure tunnels between networks (e.g., on-prem to AWS)
- **Secure Access**:
  - **SSH / RDP**: Secure remote logins
  - **Systems Manager Session Manager**: Secure, agent-based access to EC2 (no open ports required)
- **TLS Certificates**:
  - Used with CloudFront, Load Balancers, etc., to enable HTTPS

### 🛠️ Key Skills
- Design secure AWS-to-on-prem connectivity (Direct Connect + VPN)
- Enforce encryption for data-in-transit in services like:
  - **Amazon S3, DynamoDB, RDS, Redshift, CloudFront, API Gateway**
- Require HTTPS/TLS for AWS API calls
- Use Systems Manager or EC2 Instance Connect for secure admin access
- Implement secure cross-region links with private/public VIFs

---

## ✅ Task 5.2: Protect Data at Rest

### 🧠 What This Means
This is about making sure **stored data is encrypted and safe** from unauthorized access or tampering.

### 🔍 Key Concepts
- **Encryption Types**:
  - **Client-side**: You encrypt before uploading
  - **Server-side**: AWS encrypts automatically (SSE-S3, SSE-KMS, etc.)
  - **Symmetric** (same key to encrypt/decrypt) vs. **Asymmetric** (key pair)
- **Integrity Protection**:
  - Hashing (e.g., SHA256)
  - Digital signatures
- **Resource Policies**: Restrict access (e.g., S3, DynamoDB, KMS)

### 🛠️ Key Skills
- Apply encryption in services like S3, EBS, EFS, RDS, DynamoDB, SQS
- Restrict access using IAM policies and bucket/table policies
- Prevent public access using:
  - S3 Block Public Access, snapshot/AMI restrictions
- Use tools like:
  - **S3 Object Lock**, **KMS Key Policies**, **Vault Lock** to stop tampering
- Use **CloudHSM** for custom encryption in RDS or EC2-hosted databases
- Choose the right encryption type for your business case

---

## ✅ Task 5.3: Manage Data Lifecycle

### 🧠 What This Means
This is about managing how long your data lives and automating **deletion or archiving** when it's no longer needed.

### 🔍 Key Concepts
- **Lifecycle Policies**:
  - Define when to delete/archive data
- **Retention Standards**:
  - Based on compliance (e.g., keep logs 7 years)

### 🛠️ Key Skills
- Use **S3 Lifecycle Policies** to auto-transition or delete old data
- Lock data using:
  - S3 Object Lock, Glacier Vault Lock
- Automate data lifecycle in:
  - **EBS Snapshots, RDS Snapshots, AMIs, Container Images, Log Groups**
- Use **AWS Backup** to set schedules and retention rules across services

---

## ✅ Task 5.4: Protect Secrets and Keys

### 🧠 What This Means
This is about securely managing **credentials, API keys, secrets, and encryption keys**.

### 🔍 Key Concepts
- **Secrets Manager**: Store and rotate secrets like DB passwords
- **Systems Manager Parameter Store**: Store config values, secure strings
- **AWS KMS**: Create and manage encryption keys (symmetric/asymmetric)

### 🛠️ Key Skills
- Automate secret rotation (DB credentials, API keys, etc.)
- Apply tight **KMS Key Policies** to control access
- Manage import/removal of your own key material into KMS

---

## 📌 Summary Table

| Task | What You Do | AWS Tools |
|------|-------------|------------|
| 5.1 | Protect Data in Transit | TLS, VPN, Systems Manager, S3, RDS, CloudFront |
| 5.2 | Protect Data at Rest | KMS, S3, EBS, RDS, Object Lock, Vault Lock |
| 5.3 | Manage Data Lifecycle | S3 Lifecycle, AWS Backup, Data Lifecycle Manager |
| 5.4 | Protect Secrets/Keys | Secrets Manager, Parameter Store, KMS |

---

Let me know if you’d like:
- A full printable PDF with all domains
- Practice quizzes or flashcards
- Visual diagrams for this domain

