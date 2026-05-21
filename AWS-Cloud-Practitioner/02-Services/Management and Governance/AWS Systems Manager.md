# AWS Systems Manager

## What you'll learn
- What Systems Manager is and what it manages
- Key capabilities: Parameter Store, Session Manager, Patch Manager, Run Command
- Systems Manager vs manual SSH access
- Key exam facts

---

## What is AWS Systems Manager?

AWS Systems Manager is a **unified operational management service** for managing EC2 instances and on-premises servers at scale. It provides tools for patching, configuration management, software inventory, running commands, and securely accessing instances — all without needing SSH access.

---

## Key capabilities

**Parameter Store** — A secure, hierarchical storage service for configuration data and secrets. Store database connection strings, API keys, feature flags, and other configuration values. Applications retrieve parameters at runtime.
- **Standard parameters**: Free, up to 10,000 parameters
- **SecureString parameters**: Encrypted using AWS KMS
- Can be used as an alternative to AWS Secrets Manager for simpler use cases

**Session Manager** — Connect to EC2 instances via a browser-based shell or the CLI without:
- Opening SSH port (port 22) in security groups
- Managing SSH keys or bastion hosts
- Sessions are logged to S3 or CloudWatch Logs for audit purposes

**Patch Manager** — Automate the patching of EC2 instances and on-premises servers:
- Define patch baselines (which patches to apply, which to reject)
- Schedule patching windows
- Get compliance reports showing which instances are patched and which are missing patches

**Run Command** — Execute shell scripts or PowerShell commands on multiple instances simultaneously without logging in. Example: "Run this script on all EC2 instances tagged Environment=Production."

**Inventory** — Collect software inventory from managed instances — installed applications, network configurations, Windows services, and more.

**State Manager** — Define and maintain the desired state of your instances (e.g., "certain software must always be installed").

---

## Systems Manager Agent (SSM Agent)

Systems Manager requires the SSM Agent to be installed on managed instances. Many AWS-provided AMIs (Amazon Linux 2, Windows Server 2016+) come with SSM Agent pre-installed.

For Systems Manager to manage an instance, the instance also needs an IAM role with the `AmazonSSMManagedInstanceCore` policy.

---

## When to use it

**Patching at scale** — Patch hundreds of EC2 instances on a schedule without logging into each one.

**Secure instance access** — Use Session Manager instead of SSH — no open port 22, no key management, all sessions logged.

**Centralized configuration** — Store environment variables and secrets in Parameter Store; applications retrieve them at runtime.

**Bulk operations** — Use Run Command to deploy a configuration change or restart a service across a fleet of instances.

---

## Exam focus

- Systems Manager = **operational management of EC2 and on-premises** — patching, run commands, secure access, config storage
- **Parameter Store** = store and retrieve config data and secrets (alternative to Secrets Manager for simpler needs)
- **Session Manager** = access EC2 without SSH or bastion hosts
- **Patch Manager** = automated patching with compliance tracking
- **Run Command** = run scripts on multiple instances without logging in

---

## Practice questions

**Q1.** A security team wants to eliminate the need for bastion hosts and open SSH ports in their production environment. They need a way for administrators to run commands on EC2 instances securely with all sessions logged. Which AWS Systems Manager feature provides this?

A) Patch Manager  
B) Parameter Store  
C) Session Manager  
D) Run Command

**Answer: C** — Session Manager provides secure browser-based or CLI terminal access to EC2 instances without requiring port 22 to be open or SSH keys to be managed. All sessions are logged to S3 or CloudWatch. Patch Manager automates OS patching. Parameter Store stores configuration data. Run Command executes scripts across instances but is not an interactive terminal session.
