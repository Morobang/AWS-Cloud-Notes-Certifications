# Amazon GuardDuty

## What you'll learn
- What GuardDuty is and how it detects threats
- Data sources GuardDuty analyzes
- GuardDuty findings and responses
- Key exam facts

---

## What is Amazon GuardDuty?

Amazon GuardDuty is a **managed threat detection service** that continuously monitors your AWS account for malicious activity and unauthorized behavior. It uses machine learning, anomaly detection, and threat intelligence to identify potential security threats.

You enable GuardDuty with one click — no agents to install, no infrastructure to manage.

---

## What GuardDuty analyzes

GuardDuty analyzes multiple AWS data sources without affecting performance:

| Data source | What it reveals |
|-------------|----------------|
| CloudTrail logs | Unusual API activity (who is calling what APIs) |
| VPC Flow Logs | Network traffic patterns (unusual connections, data exfiltration) |
| DNS logs | DNS queries from EC2 instances (communication with known malicious domains) |
| S3 access logs | Unusual S3 data access patterns |
| EKS audit logs | Unusual Kubernetes API activity |
| Lambda network activity | Unusual network calls from Lambda functions |

---

## How GuardDuty detects threats

**Anomaly detection** — Establishes a baseline of normal activity and flags deviations (e.g., an IAM user that normally only reads S3 suddenly starts calling EC2 APIs at 3 AM).

**Threat intelligence** — GuardDuty uses threat intelligence feeds (known malicious IP addresses, domains) to identify communication with known bad actors.

**ML models** — Detect patterns associated with known attack techniques (e.g., credential exfiltration, crypto mining, port scanning).

---

## Example GuardDuty findings

- "EC2 instance is communicating with a known cryptocurrency mining pool"
- "IAM credentials are being used from an unusual geographic location"
- "EC2 instance is performing an outbound port scan"
- "S3 bucket data is being accessed from an unusual IP"

---

## Responding to findings

GuardDuty generates **findings** — each finding describes the threat, severity, and affected resource. You can:
- View findings in the GuardDuty console
- Send findings to **EventBridge** for automated responses (trigger Lambda to isolate a compromised instance)
- Send to **Security Hub** for centralized security findings

---

## Exam focus

- GuardDuty = **threat detection** — uses ML and threat intelligence to detect malicious activity
- Analyzes: **CloudTrail, VPC Flow Logs, DNS logs**
- No agent installation needed — analyzes existing AWS logs
- Generates **findings** with severity levels
- Use when exam describes: "detect compromised EC2 instances," "identify unusual API activity," "threat detection in AWS account"

---

## Practice questions

**Q1.** A security team wants to continuously monitor their AWS account for signs of compromise — such as EC2 instances connecting to malicious domains or IAM credentials being used from unusual locations. Which service provides this automated threat detection?

A) AWS Config  
B) AWS CloudTrail  
C) Amazon GuardDuty  
D) Amazon Inspector

**Answer: C** — GuardDuty continuously analyzes CloudTrail, VPC Flow Logs, and DNS logs using ML to detect these exact threat patterns. Config monitors resource configuration compliance. CloudTrail records API calls but doesn't detect threats automatically. Inspector scans EC2 for software vulnerabilities, not behavioral threats.
