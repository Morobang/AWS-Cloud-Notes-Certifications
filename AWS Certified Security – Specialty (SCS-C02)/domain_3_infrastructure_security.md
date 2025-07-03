# 🛡️ Domain 3: Infrastructure Security (AWS SCS-C02)

## ✅ Task 3.1: Edge Security Controls

### 🧠 What This Means
This is about protecting your **edge-facing services** — things like websites, APIs, and CDNs — from attacks **before they reach your core AWS infrastructure**.

### 🔍 Key Concepts
- **Common Edge Services**:
  - **AWS WAF**: Filters web traffic (e.g., block SQL injections)
  - **CloudFront**: Global CDN, works with WAF
  - **Route 53**: DNS + routing controls
  - **AWS Shield**: DDoS protection (Standard = automatic, Advanced = paid)
  - **Load Balancers**: Control and distribute incoming traffic
- **Common Threats**:
  - OWASP Top 10: SQLi, XSS, SSRF, etc.
  - DDoS: Denial of service attacks to overload your system

### 🛠️ Key Skills
- Use edge services for different app types (e.g., WAF for public sites)
- Match services to expected threats (e.g., use Shield for DDoS risk)
- Combine tools (WAF + CloudFront + Shield) for layered security
- Apply filters (e.g., block requests from certain countries or IPs)
- Turn on logging and metrics to detect unusual behavior early

---

## ✅ Task 3.2: Network Security Controls

### 🧠 What This Means
This is about **controlling how data moves through your AWS network**, who can access what, and keeping sensitive resources isolated.

### 🔍 Key Concepts
- **VPC Security Tools**:
  - **Security Groups**: Allow/deny access to instances (stateful)
  - **Network ACLs**: Firewall rules at the subnet level (stateless)
  - **AWS Network Firewall**: Advanced filtering and traffic inspection
- **Connectivity Tools**:
  - **VPC Endpoints**: Access AWS services privately
  - **Transit Gateway**: Connect VPCs and on-premises networks
  - **VPN / Direct Connect**: Secure private links to AWS
- **Telemetry**:
  - **VPC Flow Logs**, **Traffic Mirroring**, **Load Balancer logs**

### 🛠️ Key Skills
- Segment the network (e.g., separate public/private subnets)
- Control network traffic with Security Groups, NACLs, and firewalls
- Route internal traffic off the public internet using VPC endpoints
- Choose telemetry based on what you want to detect
- Ensure private connectivity is secure and redundant
- Adjust configs as environments grow (Firewall Manager)
- Remove unused open ports, IP access, etc.

---

## ✅ Task 3.3: Compute Security Controls

### 🧠 What This Means
This section is about **protecting EC2 instances, containers, and related resources** in your AWS compute environment.

### 🔍 Key Concepts
- **EC2 & AMI Security**:
  - Patch, inspect, and use secure AMIs (via EC2 Image Builder)
- **IAM Roles**:
  - Assign least-privilege IAM roles to EC2 and Lambda
- **Scanning & Hardening**:
  - Use **Amazon Inspector** to scan for vulnerabilities
  - Use firewalls and OS hardening to secure hosts

### 🛠️ Key Skills
- Build secure AMIs with updated software and config
- Assign roles (no hardcoded credentials!)
- Run scans on EC2 and containers for known issues
- Patch instances at scale
- Analyze Inspector findings and take action
- Pass secrets securely (e.g., via Secrets Manager)

---

## ✅ Task 3.4: Troubleshoot Network Security

### 🧠 What This Means
If there's a network problem — a system isn't reachable, a port is closed, or access is broken — you need to **figure out what’s wrong** and fix it.

### 🔍 Key Concepts
- **Troubleshooting Tools**:
  - **Reachability Analyzer**: Map how packets flow between services
  - **VPC Flow Logs**: Show who’s talking to whom and if it's allowed
  - **Traffic Mirroring**: Capture live packets to analyze traffic
- **Networking Knowledge**:
  - TCP vs UDP, ports, OSI model
  - Network utilities like `ping`, `traceroute`, etc.

### 🛠️ Key Skills
- Use logs to trace blocked or failed traffic
- Find misconfigured security groups or NACLs
- Capture and analyze traffic samples
- Interpret Inspector network findings
- Fix misrouted or blocked connections based on log data

---

## 📌 Summary Table

| Task | What You Do | AWS Tools |
|------|-------------|------------|
| 3.1 | Edge Security | WAF, CloudFront, Route 53, Shield |
| 3.2 | Network Security | Security Groups, NACLs, Network Firewall, VPC Endpoints |
| 3.3 | Compute Security | EC2, IAM roles, Inspector, Image Builder |
| 3.4 | Troubleshoot Network | Reachability Analyzer, VPC Flow Logs, Traffic Mirroring |

---

Let me know if you’d like practice questions, a quiz, or visuals added to this file!

