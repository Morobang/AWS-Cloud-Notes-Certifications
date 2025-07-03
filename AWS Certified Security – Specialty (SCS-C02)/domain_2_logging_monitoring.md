# 📊 Domain 2: Security Logging and Monitoring (AWS SCS-C02)

## ✅ Task 2.1: Design and Implement Monitoring and Alerting

### 🧠 What This Means
You need to make sure your AWS environment is being watched for any unusual or harmful activity. If something goes wrong, your system should be able to **notify you automatically**.

### 🔍 Key Concepts
- **Monitoring Tools**:
  - **CloudWatch**: Monitors performance and sets alarms.
  - **EventBridge**: Triggers actions based on events.
  - **Lambda & SNS**: Automate responses and send notifications.
  - **Security Hub**: Central dashboard for security alerts.
  - **GuardDuty & Systems Manager**: Detect unusual activity and baseline behavior.

### 🛠️ Key Skills
- Understand your system architecture and decide what needs to be monitored.
- Design monitoring rules based on business and security needs.
- Use **custom insights** in Security Hub for regular security checks.
- Define metrics (e.g., failed login attempts) and thresholds for alerts.

---

## ✅ Task 2.2: Troubleshoot Monitoring and Alerting

### 🧠 What This Means
Sometimes alerts don’t work or don’t appear when they should. You must figure out why monitoring failed and fix it.

### 🔍 Key Concepts
- Make sure monitoring services (like Security Hub) are correctly set up.
- Know what log data to expect during a security event.

### 🛠️ Key Skills
- Analyze permissions and settings if an alert didn’t trigger.
- Fix misconfigured apps or services that don’t send monitoring data.
- Ensure all systems meet monitoring standards.

---

## ✅ Task 2.3: Design and Implement a Logging Solution

### 🧠 What This Means
You must make sure your AWS services are **generating logs**, storing them safely, and keeping them long enough for analysis.

### 🔍 Key Concepts
- **Logging Services**:
  - **CloudTrail**: Tracks AWS API calls.
  - **CloudWatch Logs**: Captures app/service logs.
  - **VPC Flow Logs**: Monitors network traffic.
  - **Route 53 DNS Logs**: Tracks DNS queries.
- **Log Attributes**: Include log level (info, error), format, and verbosity.
- **Storage & Lifecycle**:
  - Set how long logs should be kept.
  - Move old logs to Glacier or archive them.

### 🛠️ Key Skills
- Turn on logging for AWS services.
- Identify which services and apps should log data.
- Set retention rules and organize log storage.

---

## ✅ Task 2.4: Troubleshoot Logging Solutions

### 🧠 What This Means
If logs are missing or incomplete, you need to diagnose the issue and fix it.

### 🔍 Key Concepts
- Understand **log details** like verbosity, timing, immutability.
- Know which permissions are required for logging to work.

### 🛠️ Key Skills
- Detect missing permissions (e.g., S3 bucket doesn’t allow writing logs).
- Fix configuration issues that prevent logging.
- Find out why a log stream is empty or missing.

---

## ✅ Task 2.5: Design a Log Analysis Solution

### 🧠 What This Means
Once logs are collected, you need to **analyze** them for anomalies, threats, or trends.

### 🔍 Key Concepts
- **Analysis Tools**:
  - **Athena**: Run SQL queries on logs stored in S3.
  - **CloudWatch Logs Insights**: Interactive log searching.
  - **CloudTrail Insights / Security Hub Insights**: Detect unusual behavior.
- **Log Format**:
  - CloudTrail logs include user, action, time, and source.

### 🛠️ Key Skills
- Spot unusual patterns (e.g., repeated failed logins).
- Normalize and combine logs from different services.
- Correlate events to find the root of a security issue.

---

## 📌 Summary Table

| Task | What You Do | AWS Tools |
|------|-------------|------------|
| 2.1 | Monitor & Alert | CloudWatch, EventBridge, SNS, Security Hub |
| 2.2 | Fix Alert Failures | Security Hub, IAM Permissions |
| 2.3 | Logging Solution | CloudTrail, CloudWatch Logs, VPC Flow Logs |
| 2.4 | Fix Log Issues | IAM, S3 Bucket Policies, Log Configs |
| 2.5 | Analyze Logs | Athena, Logs Insights, Security Hub |

---

Let me know if you'd like a printable version, quiz, or flashcards for this domain!

