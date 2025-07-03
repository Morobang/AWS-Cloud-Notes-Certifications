# 🛡️ Domain 1: Threat Detection and Incident Response (AWS SCS-C02)

## ✅ Task 1.1: Design and Implement an Incident Response Plan

### 🌩️ What Is Incident Response?
Incident response is how you **detect, investigate, and fix security issues** in the cloud.

> Think of it like your home security system — when something goes wrong, you need a plan to figure out what happened, lock things down, and prevent it from happening again.

### 🔍 Key Knowledge
- **AWS Best Practices**: Use the [AWS Security Incident Response Guide](https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/aws-security-incident-response-guide.html).
- **Cloud Incidents**: Credential leaks, data theft, unauthorized access, malware.
- **Roles**:
  - Security team: Investigate and contain
  - DevOps: Help isolate and fix
  - Management: Approve and communicate
- **ASFF (AWS Security Finding Format)**: Standard JSON format for findings across AWS tools.

### 🛠️ Skills
- **Credential Rotation**:
  - IAM to revoke access
  - Secrets Manager to rotate secrets
- **Resource Isolation**:
  - Quarantine with security groups or isolated VPC
  - Stop EC2 instances
- **Playbooks and Runbooks**:
  - Define steps for each type of incident
- **Use of Security Services**:
  - Security Hub, GuardDuty, Macie, Inspector, AWS Config, Detective, IAM Access Analyzer
- **Automation with EventBridge + ASFF**:
  - Event-driven response (e.g., trigger Lambda on GuardDuty finding)

---

## ✅ Task 1.2: Detect Security Threats and Anomalies

### 🔍 Key Knowledge
- **Detection Tools**:
  - GuardDuty: Behavioral and threat detection
  - Macie: Sensitive data classification
  - Config: Policy and change tracking
  - Inspector: Vulnerability assessment
  - IAM Access Analyzer: Identifies unintended public access
  - Security Hub: Centralized dashboard
- **Anomaly Detection**:
  - CloudWatch: Metric filters and dashboards
  - Athena: Query logs in S3
  - Detective: Pivot and correlate data
- **Centralization**:
  - Use Security Hub and ASFF to combine findings

### 🛠️ Skills
- **Evaluate Findings**: Understand severity, resource affected, and action required
- **Cross-Service Correlation**: Connect alerts (e.g., GuardDuty + Macie = data exfiltration)
- **Query Logs**: Use Athena to investigate incidents
- **Create Dashboards**: Use CloudWatch to visualize threats

---

## ✅ Task 1.3: Respond to Compromised Resources and Workloads

### 🔍 Key Knowledge
- **AWS Security IR Guide**: Standard process for incident management
- **Isolation Techniques**: Remove access, move to separate VPC, change security group
- **Root Cause Analysis**:
  - Use Detective, CloudTrail, Athena
- **Forensics and Logging**:
  - Capture snapshots, memory dumps, log data
  - Use S3 Object Lock to protect evidence

### 🛠️ Skills
- **Automate Remediation**: Lambda, Step Functions, Systems Manager Runbooks
- **Isolate EC2**: Stop instance, detach from network, modify permissions
- **Investigate RCA**: Use Detective, Athena, log correlation
- **Preserve Evidence**: S3 Object Lock, S3 Replication, Lifecycle policies
- **Recovery Planning**: Restore from backup, test response process

---

## 📌 Summary Table

| Task | What You Do | Key AWS Tools |
|------|-------------|----------------|
| **1.1** | Plan Response | IAM, Secrets Manager, EventBridge, Security Hub |
| **1.2** | Detect Threats | GuardDuty, Macie, Athena, CloudWatch |
| **1.3** | Respond | Lambda, Systems Manager, Detective, Snapshots |

