# **AWS Infrastructure Security (SCS-C02 - Domain 3) - Simplified Notes**

## **1. Edge Security (Protecting the Front Door)**
### **What is it?**
- Protects your **public-facing services** (websites, APIs, CDNs) from attacks before they reach your servers.

### **Key AWS Services:**
| Service | What It Does | Example |
|---------|-------------|---------|
| **AWS WAF** | Blocks malicious web traffic (SQL injection, XSS) | Stops hackers from exploiting a login form |
| **CloudFront (CDN)** | Speeds up & secures content delivery globally | Protects a blog from being overloaded |
| **AWS Shield** | Prevents DDoS attacks (flooding your site with fake traffic) | Keeps an online store running during an attack |
| **Route 53 (DNS)** | Controls website routing & filters bad traffic | Blocks visitors from high-risk countries |

### **Why It Matters:**
- Without edge security, hackers can:
  - Crash your website (DDoS)
  - Steal data (SQL injection)
  - Deface your site (XSS)

### **How to Remember:**
- **WAF** = Web Application Firewall (like a bouncer checking IDs)
- **Shield** = Anti-DDoS (like a shield blocking arrows)
- **CloudFront** = Fast, secure content delivery (like a highway with toll booths)

---

## **2. Network Security (Locking Down Internal Traffic)**
### **What is it?**
- Controls **who can access what** inside your AWS network (VPCs, subnets, databases).

### **Key AWS Services:**
| Service | What It Does | Example |
|---------|-------------|---------|
| **Security Groups** | Firewall for EC2 instances (allows/denies traffic) | Only lets your IP access SSH |
| **NACLs (Network ACLs)** | Extra firewall at subnet level | Blocks all traffic except HTTP/HTTPS |
| **VPC Endpoints** | Private AWS access (no public internet) | Lets EC2 securely talk to S3 |
| **VPC Flow Logs** | Records network traffic for investigation | Finds suspicious login attempts |

### **Why It Matters:**
- Stops hackers from moving sideways in your network.
- Prevents accidental exposure of databases/servers.

### **How to Remember:**
- **Security Groups** = Apartment door locks (instance-level)
- **NACLs** = Building security (subnet-level)
- **Flow Logs** = Security camera footage (see who entered)

---

## **3. Compute Security (Protecting Servers & Apps)**
### **What is it?**
- Secures **EC2 instances, containers, and serverless apps**.

### **Key AWS Services:**
| Service | What It Does | Example |
|---------|-------------|---------|
| **IAM Roles** | Grants permissions to AWS services (no passwords!) | Lets Lambda read from S3 |
| **Amazon Inspector** | Scans for vulnerabilities (unpatched software) | Finds a server missing security updates |
| **EC2 Image Builder** | Creates secure, pre-configured server images (AMIs) | Auto-builds patched AMIs monthly |

### **Why It Matters:**
- Prevents servers from being hacked due to weak permissions or outdated software.
- Automates security best practices.

### **How to Remember:**
- **IAM Roles** = Keys without passwords (safer than hardcoded credentials)
- **Inspector** = A doctor checking servers for "illnesses" (vulnerabilities)

---

## **4. Troubleshooting (Finding & Fixing Security Issues)**
### **What is it?**
- Tools to **diagnose why something isn’t working** (e.g., "Why can’t my EC2 reach the internet?").

### **Key AWS Services:**
| Service | What It Does | Example |
|---------|-------------|---------|
| **Reachability Analyzer** | Tests if resources can communicate | Checks if a Security Group blocks RDS |
| **VPC Flow Logs** | Shows allowed/denied network traffic | Finds a hacker probing your servers |
| **Traffic Mirroring** | Captures live traffic for analysis | Debugs a broken API connection |

### **Why It Matters:**
- Quickly finds misconfigurations before they cause breaches.
- Helps audit security controls.

### **How to Remember:**
- **Reachability Analyzer** = A network "ping" test
- **Flow Logs** = A logbook of who visited your network

---

## **📌 Summary Cheat Sheet**
| **Security Area** | **Purpose** | **Top AWS Tools** |
|------------------|------------|------------------|
| **Edge Security** | Protect websites/APIs | WAF, CloudFront, Shield |
| **Network Security** | Control internal traffic | Security Groups, NACLs, VPC Endpoints |
| **Compute Security** | Secure servers & apps | IAM Roles, Inspector, Image Builder |
| **Troubleshooting** | Fix network issues | Reachability Analyzer, Flow Logs |

---

## **🎓 Study Tips**
1. **Use Analogies** (e.g., WAF = bouncer, NACLs = building security).
2. **Hands-On Practice** (try blocking an IP in WAF).
3. **Flashcards** (Q: "What stops DDoS?" A: **Shield**).
4. **Scenarios** (e.g., "Your website is slow—what do you check?").

Would you like **practice questions** or a **diagram** next? 🚀