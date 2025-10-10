# Task Statement 2.1: Understand the AWS Shared Responsibility Model

## 🎯 **Learning Objective**
**Master the AWS Shared Responsibility Model** - understand exactly who (AWS or you) is responsible for each aspect of cloud security, and how this changes based on different AWS services.

---

## 🏠 **Understanding Through Real-World Analogies**

### **The Apartment Complex Analogy**
The AWS Shared Responsibility Model is like living in an apartment complex:

#### **Building Management (AWS) Responsibilities:**
```
What the apartment complex provides and maintains:
├── Building Security
│   ├── Secure building entrance with keypad/doorman
│   ├── Security cameras in hallways and parking
│   └── Background checks on maintenance staff
├── Infrastructure  
│   ├── Working elevators and stairwells
│   ├── Electrical and plumbing systems
│   ├── HVAC (heating and cooling)
│   └── Fire safety systems and sprinklers
├── Facility Maintenance
│   ├── Roof repairs and structural maintenance  
│   ├── Hallway cleaning and lighting
│   ├── Parking lot maintenance
│   └── Garbage collection systems
└── Compliance
    ├── Building permits and inspections
    ├── Fire department safety compliance
    └── Meeting local building codes
```

#### **Tenant (Your) Responsibilities:**
```
What you're responsible for in your apartment:
├── Your Unit Security
│   ├── Lock your apartment door
│   ├── Don't give keys to unauthorized people
│   ├── Secure your valuables inside your unit
│   └── Report suspicious activity
├── Your Property
│   ├── Your furniture and belongings
│   ├── Your personal documents and information
│   ├── Care for your appliances
│   └── Keep your unit clean and undamaged
├── Following Rules
│   ├── Follow building policies and lease agreement
│   ├── Use facilities appropriately
│   ├── Don't disrupt other tenants
│   └── Notify management of maintenance issues
└── Your Guests
    ├── Control who you let into your apartment
    ├── Ensure your guests follow building rules
    └── Take responsibility for guest behavior
```

**Key insight:** You can't just assume the building management will protect everything - you still need to lock your door and secure your belongings!

---

## 🔒 **The AWS Shared Responsibility Model Explained**

### **What is the Shared Responsibility Model?**
**Definition:** A security and compliance framework that clearly defines which security tasks are AWS's responsibility and which are the customer's responsibility.

**Why it exists:**
- **Prevents gaps**: Ensures no security task falls through the cracks
- **Clarifies expectations**: Everyone knows exactly what they're responsible for
- **Enables better security**: Both parties can focus on their areas of expertise
- **Reduces confusion**: Clear guidelines for who to contact when issues arise

### **The Two Main Categories**

#### **AWS: "Security OF the Cloud"**
AWS is responsible for protecting the infrastructure that runs all services offered in the AWS Cloud.

**What this includes:**
```
AWS's Infrastructure Responsibilities:
├── Physical Security
│   ├── Data center physical access controls
│   ├── Environmental controls (temperature, humidity)
│   ├── Fire suppression and power backup systems
│   └── Hardware disposal and destruction
├── Infrastructure Software
│   ├── Host operating system patching
│   ├── Network infrastructure configuration
│   ├── Hypervisor (virtualization layer) management
│   └── Service software updates and patches
├── Network Controls
│   ├── Network segregation between customers
│   ├── DDoS protection at infrastructure level
│   ├── Port scanning protection
│   └── Network packet filtering
└── Hardware/Software Maintenance
    ├── Server hardware maintenance and replacement
    ├── Network equipment maintenance
    ├── Storage system management
    └── Database software patching (for managed services)
```

**Real-world analogy:** Like a hotel managing the building structure, utilities, and basic safety systems that all guests rely on.

#### **Customer: "Security IN the Cloud"**
Customers are responsible for security tasks that relate to their specific use of AWS services.

**What this includes:**
```
Your Security Responsibilities:
├── Data Protection
│   ├── Data encryption (at rest and in transit)
│   ├── Data classification and handling
│   ├── Data backup and disaster recovery
│   └── Data retention and deletion policies
├── Identity & Access Management
│   ├── User account management
│   ├── Multi-factor authentication setup
│   ├── Permission and role assignments
│   └── Access key rotation and security
├── Operating System & Applications
│   ├── Operating system updates and patches
│   ├── Application security configurations
│   ├── Anti-virus and anti-malware software
│   └── Application firewall configurations
├── Network Configuration
│   ├── Security group configurations
│   ├── Network Access Control Lists (NACLs)
│   ├── VPC (Virtual Private Cloud) setup
│   └── Subnet configurations
└── Compliance
    ├── Meeting industry-specific regulations
    ├── Configuring audit logging
    ├── Implementing required security controls
    └── Regular security assessments
```

**Real-world analogy:** Like a hotel guest being responsible for locking their room door, securing their valuables, and following hotel policies.

---

## 📊 **How Responsibility Changes by Service Type**

### **The Service Spectrum**
Different AWS services require different levels of customer responsibility:

```
More AWS Responsibility ←→ More Customer Responsibility
        |                           |
   SaaS-like           IaaS-like Services
   Services           (Infrastructure)
        |                           |
   (Less work          (More control,
    for you)            more work)
```

### **1. Infrastructure as a Service (IaaS) - Most Customer Responsibility**

#### **Example: Amazon EC2 (Virtual Servers)**
**Think of this like:** Renting a raw apartment - you get the space, but you provide and manage everything inside.

**AWS Responsibilities:**
- Physical server hardware
- Hypervisor (virtualization software)
- Network infrastructure
- Physical data center security

**Your Responsibilities:**
- Operating system installation and updates
- Application installation and configuration
- Data encryption
- Network firewall configuration
- Access management
- Security patches
- Anti-virus software

**Real-world comparison:**
```
Traditional Server (Your Own Building):
You're responsible for: Building, server hardware, OS, applications, data
                      [═══════════════════════════════════════════]

AWS EC2 (Rented Server Space):
AWS responsible for: [████████████] Building, hardware, hypervisor
You responsible for:                [═════════════════════════] OS, applications, data
```

### **2. Platform as a Service (PaaS) - Shared Responsibility**

#### **Example: Amazon RDS (Managed Database)**
**Think of this like:** Renting a furnished apartment - furniture (database software) is provided and maintained, but you manage your belongings (data).

**AWS Responsibilities:**
- Database software installation and patching
- Operating system maintenance
- Hardware management
- Backup infrastructure
- Automatic failover

**Your Responsibilities:**
- Database access controls
- Data encryption
- User account management
- Query optimization
- Database-level security configurations

**Real-world comparison:**
```
Self-Managed Database:
You're responsible for: [═══════════════════════════════════════════]
                       Hardware, OS, DB software, data, backups

AWS RDS Managed Database:
AWS responsible for: [████████████████████████] Hardware, OS, DB software, backups
You responsible for:                            [═══════════] Data, access controls, encryption
```

### **3. Software as a Service (SaaS) - Least Customer Responsibility**

#### **Example: Amazon WorkMail (Email Service)**
**Think of this like:** Using a hotel's business center - the hotel provides and maintains the computers and software, you just use them for your work.

**AWS Responsibilities:**
- Email server software and maintenance
- Anti-spam and anti-virus protection
- Infrastructure scaling
- Software updates
- Data backup

**Your Responsibilities:**
- User account management
- Email content security
- Access policies
- Data classification

**Real-world comparison:**
```
Self-Hosted Email:
You're responsible for: [═══════════════════════════════════════════]
                       Mail servers, software, maintenance, security

AWS WorkMail:
AWS responsible for: [██████████████████████████████████] Servers, software, maintenance
You responsible for:                                     [═══] User management, content
```

---

## 🔧 **Practical Examples by AWS Service**

### **Amazon S3 (Simple Storage Service)**
**Service Type:** Platform as a Service (PaaS)

**AWS Handles:**
- Storage infrastructure
- Data durability (11 9's = 99.999999999%)
- Infrastructure scaling
- Physical security of data centers
- Service availability

**You Handle:**
- **Bucket permissions** - who can access your storage buckets
- **Data encryption settings** - choosing encryption options
- **Access logging** - tracking who accesses your data
- **Data classification** - marking sensitive vs. public data
- **Backup policies** - deciding what to backup and when

**Real-world scenario:**
```
Scenario: Company storing customer photos in S3
├── AWS ensures: Photos don't get lost due to hardware failure
├── AWS ensures: Data center is physically secure
├── You ensure: Only authorized employees can access the photos  
├── You ensure: Customer photos are encrypted
└── You ensure: Access to photos is logged for compliance
```

### **Amazon EC2 (Elastic Compute Cloud)**
**Service Type:** Infrastructure as a Service (IaaS)

**AWS Handles:**
- Physical server hardware
- Hypervisor security
- Network infrastructure
- Physical data center access

**You Handle:**
- **Operating system updates** - keeping Windows/Linux updated
- **Security software** - installing antivirus, endpoint protection
- **Application security** - securing your web applications
- **Network configuration** - setting up firewalls and access rules
- **Data encryption** - encrypting files and databases
- **User access management** - controlling who can log into servers

**Real-world scenario:**
```
Scenario: Company running e-commerce website on EC2
├── AWS ensures: Physical servers are secure and maintained
├── AWS ensures: Network infrastructure is protected
├── You ensure: Web server software is updated and secure
├── You ensure: Customer data in databases is encrypted
├── You ensure: Only authorized staff can access the servers
└── You ensure: Firewall rules block malicious traffic
```

### **AWS Lambda (Serverless Functions)**
**Service Type:** Platform as a Service (PaaS)

**AWS Handles:**
- Server provisioning and management
- Runtime environment patching
- Infrastructure scaling
- Runtime security

**You Handle:**
- **Function code security** - writing secure application code
- **Access permissions** - controlling what the function can access
- **Environment variables** - securing configuration data
- **Input validation** - checking data coming into your function

**Real-world scenario:**
```
Scenario: Function that processes customer orders
├── AWS ensures: The servers running your function are secure
├── AWS ensures: The runtime environment (Python, Node.js) is patched
├── You ensure: Your order processing code handles data securely
├── You ensure: Function only has permissions it needs
└── You ensure: Customer data is validated and encrypted
```

---

## 🚨 **Common Responsibility Confusion Points**

### **1. Data Encryption**
**Confusion:** "Isn't AWS responsible for keeping my data secure?"

**Reality Check:**
- **AWS responsibility**: Providing encryption options and tools
- **Your responsibility**: Actually turning on and configuring encryption

**Analogy:** Hotel provides a safe in your room, but you have to:
- Choose to use it
- Set the combination
- Decide what to put in it

### **2. Operating System Updates**
**Confusion:** "I thought managed services meant AWS handles everything?"

**Reality Check:**
- **For managed services (RDS, ElastiCache)**: AWS handles OS updates
- **For infrastructure services (EC2)**: You handle OS updates
- **Rule of thumb**: If you can access the operating system, you're responsible for it

### **3. Access Management**
**Confusion:** "AWS has security, so I don't need to worry about access control?"

**Reality Check:**
- **AWS responsibility**: Providing access management tools (IAM)  
- **Your responsibility**: Actually configuring who can access what

**Analogy:** Building management provides key cards, but you decide:
- Who gets keys to your office
- What rooms each person can access  
- When to revoke access for ex-employees

### **4. Compliance**
**Confusion:** "AWS is compliant, so my applications are automatically compliant too?"

**Reality Check:**
- **AWS responsibility**: AWS infrastructure meets compliance standards
- **Your responsibility**: Configuring your applications to meet compliance requirements

**Example:** For HIPAA compliance:
- **AWS provides**: HIPAA-eligible services and infrastructure
- **You must**: Enable encryption, set up audit logging, implement access controls, sign Business Associate Agreement

---

## 🛠️ **The Shared Responsibility Matrix**

### **By Service Category**

| Security Component | EC2 (IaaS) | RDS (PaaS) | S3 (PaaS) | Lambda (PaaS) |
|-------------------|------------|------------|-----------|---------------|
| **Physical Security** | AWS | AWS | AWS | AWS |
| **Host OS Patching** | Customer | AWS | AWS | AWS |
| **Guest OS Patching** | Customer | N/A | N/A | N/A |
| **Application Patching** | Customer | Customer | N/A | Customer |
| **Network Configuration** | Customer | AWS/Customer | Customer | AWS |
| **Firewall Settings** | Customer | Customer | Customer | AWS |
| **Data Encryption** | Customer | Customer | Customer | Customer |
| **Access Management** | Customer | Customer | Customer | Customer |

**Legend:**
- **AWS**: AWS handles this completely
- **Customer**: You handle this completely  
- **AWS/Customer**: Shared between both
- **N/A**: Not applicable for this service

---

## 📋 **Security Responsibility Checklist**

### **For ANY AWS Service, You're ALWAYS Responsible For:**
```
✓ Identity and Access Management (IAM)
  ├── Creating and managing user accounts
  ├── Setting up appropriate permissions
  ├── Enabling multi-factor authentication
  └── Regularly reviewing and updating access

✓ Data Protection
  ├── Choosing appropriate encryption settings
  ├── Classifying data sensitivity levels
  ├── Implementing backup strategies
  └── Ensuring proper data handling procedures

✓ Network Security Configuration
  ├── Security group rules (firewall settings)
  ├── Network Access Control Lists (NACLs)
  ├── VPC configuration and subnets
  └── SSL/TLS certificate management

✓ Compliance Implementation
  ├── Understanding applicable regulations
  ├── Configuring required security controls
  ├── Enabling appropriate logging and monitoring
  └── Regular compliance assessments
```

### **For Infrastructure Services (EC2), You're ALSO Responsible For:**
```
✓ Operating System Security
  ├── Installing security updates and patches
  ├── Configuring OS-level security settings
  ├── Installing and managing antivirus software
  └── Managing system users and permissions

✓ Application Security
  ├── Keeping applications updated
  ├── Configuring application security settings
  ├── Managing application user accounts
  └── Implementing secure coding practices

✓ Host-Based Firewalls
  ├── Configuring firewall rules on the server
  ├── Managing intrusion detection systems
  ├── Monitoring system logs for security events
  └── Implementing endpoint protection
```

---

## 🎯 **Decision Framework: "Who's Responsible?"**

When you encounter a security task, ask these questions:

### **Question 1: Can I directly control or configure this?**
- **YES** → Probably your responsibility
- **NO** → Probably AWS's responsibility

**Examples:**
- Can you change database software version? **NO** (in RDS) → AWS responsibility
- Can you configure user permissions? **YES** → Your responsibility

### **Question 2: Does this relate to AWS infrastructure or my data/applications?**
- **AWS Infrastructure** → AWS responsibility
- **Your Data/Applications** → Your responsibility

**Examples:**
- Physical data center security → AWS responsibility
- Encrypting your customer data → Your responsibility

### **Question 3: What service type am I using?**
- **More managed (SaaS-like)** → More AWS responsibility
- **Less managed (IaaS-like)** → More your responsibility

**Examples:**
- WorkMail (managed email) → AWS handles most security
- EC2 (virtual servers) → You handle most security

---

## 🔍 **Real-World Scenarios and Solutions**

### **Scenario 1: Small Business Website**
**Setup:** Company website running on EC2 with customer data in RDS database

**Security Incident:** Website gets hacked and customer data is exposed

**Responsibility Analysis:**
```
What went wrong and who's responsible:

Physical server compromise?
├── If: AWS data center was breached → AWS responsible
└── If: Application vulnerability exploited → Customer responsible

Operating system compromise?  
├── If: Hypervisor vulnerability → AWS responsible
└── If: Unpatched OS on EC2 → Customer responsible

Database access compromise?
├── If: RDS service vulnerability → AWS responsible  
├── If: Weak database passwords → Customer responsible
└── If: Missing encryption → Customer responsible

Application code vulnerability?
└── Application security flaws → Customer responsible
```

**Prevention Steps (Customer Responsibilities):**
1. **Keep EC2 operating system updated** with security patches
2. **Configure security groups** to limit access to necessary ports only
3. **Enable RDS encryption** for customer data protection
4. **Use strong database passwords** and rotate them regularly
5. **Implement application-level security** (input validation, secure coding)
6. **Enable CloudTrail logging** to track all account activities

### **Scenario 2: Healthcare Application**
**Setup:** Patient data stored in S3, application running on Lambda functions

**Compliance Requirement:** Must meet HIPAA requirements for patient data protection

**Responsibility Breakdown:**
```
HIPAA Compliance Requirements:

AWS Responsibilities:
├── Provide HIPAA-eligible services (S3, Lambda)
├── Sign Business Associate Agreement (BAA)
├── Maintain infrastructure security controls
└── Provide audit reports for AWS infrastructure

Customer Responsibilities:  
├── Enable S3 encryption for patient data
├── Configure appropriate access controls and permissions
├── Implement audit logging (CloudTrail, S3 access logs)
├── Ensure Lambda functions handle patient data securely
├── Create data backup and retention policies
├── Train staff on HIPAA requirements
└── Conduct regular security assessments
```

**Key Point:** AWS provides the tools and infrastructure for HIPAA compliance, but you must actually implement and configure the required security controls.

### **Scenario 3: Financial Services Application**
**Setup:** Trading application with high-frequency data processing

**Regulatory Requirement:** Must meet SOX (Sarbanes-Oxley) financial reporting requirements

**Security Implementation:**
```
SOX Compliance Implementation:

Data Integrity Controls:
├── AWS provides: Reliable, durable storage infrastructure
├── Customer implements: Data validation, change management processes
├── Customer implements: Audit trails for all data changes
└── Customer implements: Segregation of duties in application access

Access Controls:
├── AWS provides: IAM service and MFA capabilities
├── Customer implements: Role-based access control
├── Customer implements: Regular access reviews and certifications
└── Customer implements: Strong authentication requirements

Financial Data Protection:
├── AWS provides: Encryption services and secure infrastructure  
├── Customer implements: Data encryption at rest and in transit
├── Customer implements: Secure key management practices
└── Customer implements: Data loss prevention controls
```

---

## 📊 **Understanding Through Visual Models**

### **The Shared Responsibility Stack**
```
                     YOUR APPLICATIONS AND DATA
                    ┌─────────────────────────────┐
                    │     Your Responsibility     │
                    │ ┌─────────────────────────┐ │
                    │ │  Data & Configuration   │ │
                    │ │  Identity & Access Mgmt │ │
                    │ │  Application Security   │ │
                    │ │  OS & Network Config    │ │ ← Varies by service
                    │ └─────────────────────────┘ │
                    └─────────────────────────────┘
                             ┌─────────┐
                             │ Shared  │
                             └─────────┘
                    ┌─────────────────────────────┐
                    │     AWS Responsibility      │
                    │ ┌─────────────────────────┐ │
                    │ │   Physical Security     │ │
                    │ │   Infrastructure        │ │
                    │ │   Network Controls      │ │
                    │ │   Host OS & Hypervisor  │ │
                    │ └─────────────────────────┘ │
                    └─────────────────────────────┘
                       AWS GLOBAL INFRASTRUCTURE
```

### **The Responsibility Gradient**
```
Customer Control & Responsibility
        ↑
        │
High    │  ┌──────────┐
        │  │   EC2    │  ← You manage OS, apps, data
        │  │  (IaaS)  │
        │  └──────────┘
        │
Medium  │  ┌──────────┐
        │  │   RDS    │  ← AWS manages OS, you manage data
        │  │  (PaaS)  │
        │  └──────────┘
        │
Low     │  ┌──────────┐
        │  │WorkMail  │  ← AWS manages most, you manage users
        │  │  (SaaS)  │
        │  └──────────┘
        │
        └─────────────────────────────→
                                 AWS Management & Responsibility
```

---

## ⚠️ **Common Mistakes and How to Avoid Them**

### **Mistake 1: "AWS Handles All Security"**
**Wrong Assumption:** "Since AWS is secure, I don't need to worry about security."

**Reality Check:** AWS secures the infrastructure, but you must secure your applications and data.

**How to Avoid:**
- Always ask: "What security tasks are MY responsibility?"
- Review AWS security best practices for each service you use
- Implement security controls for your specific applications and data

### **Mistake 2: "I'm Responsible for Everything"**
**Wrong Assumption:** "I need to secure everything from the physical hardware up."

**Reality Check:** AWS handles infrastructure security so you can focus on application and data security.

**How to Avoid:**
- Understand what AWS already provides (don't duplicate effort)
- Focus your security efforts on areas you actually control
- Leverage AWS security services instead of building everything yourself

### **Mistake 3: "Managed Services Mean No Security Work"**
**Wrong Assumption:** "RDS is managed, so AWS handles all database security."

**Reality Check:** AWS manages the database software and infrastructure, but you still control access, encryption, and data protection.

**How to Avoid:**
- Understand that "managed" doesn't mean "fully automated security"
- Learn what security configurations you still need to handle
- Don't assume default settings are appropriate for your security needs

### **Mistake 4: "Security Responsibility Never Changes"**
**Wrong Assumption:** "The shared responsibility model is the same for all services."

**Reality Check:** Your responsibilities change significantly based on which AWS services you use and how you use them.

**How to Avoid:**
- Review the shared responsibility model for each new AWS service
- Understand how service combinations affect your responsibilities  
- Regularly reassess responsibilities as you add or change services

---

## 📚 **Study Guide for Exam Success**

### **Key Points to Memorize**

#### **AWS ALWAYS Responsible For:**
- Physical security of data centers
- Hardware maintenance and replacement
- Network infrastructure security
- Host operating system patching (except EC2)
- Hypervisor security

#### **Customer ALWAYS Responsible For:**
- Identity and Access Management (IAM)
- Data encryption and protection
- Network configuration (security groups, NACLs)
- Application-level security
- Compliance implementation

#### **Depends on Service Type:**
- Operating system patching (Customer for EC2, AWS for managed services)
- Application patching (Customer for custom apps, AWS for managed apps)
- Network controls (varies by service complexity)

### **Exam Question Patterns**

#### **Pattern 1: Responsibility Assignment**
**Question:** "Who is responsible for patching the guest operating system on EC2 instances?"
**Answer:** Customer (because you control the OS on EC2)

#### **Pattern 2: Service Comparison**
**Question:** "What's the difference in security responsibilities between EC2 and RDS?"
**Answer:** EC2 = you patch OS; RDS = AWS patches DB software and OS

#### **Pattern 3: Compliance Scenarios**
**Question:** "For HIPAA compliance, what must the customer implement?"
**Answer:** Encryption, access controls, audit logging (AWS provides the tools, customer implements the controls)

#### **Pattern 4: Security Incident Analysis**
**Question:** "A company's website was hacked through an unpatched vulnerability. Who is responsible?"
**Answer:** Depends on where the vulnerability was - application (customer), OS on EC2 (customer), AWS infrastructure (AWS)

---

## 🎯 **Real-World Application Tips**

### **When Planning a New AWS Project**
1. **List all AWS services** you plan to use
2. **Review shared responsibility** for each service  
3. **Create security checklist** of your responsibilities
4. **Budget time and resources** for security tasks you must handle
5. **Plan security training** for team members who will manage these responsibilities

### **When Responding to Security Incidents**
1. **Identify the affected component** (application, OS, AWS infrastructure)
2. **Check the shared responsibility model** for that component
3. **Contact appropriate party** (your team for customer responsibilities, AWS Support for AWS responsibilities)
4. **Document lessons learned** to prevent similar incidents

### **When Implementing Compliance Requirements**
1. **Understand what AWS provides** (infrastructure compliance, audit reports)
2. **Identify what you must implement** (encryption, access controls, logging)
3. **Use AWS compliance resources** (whitepapers, compliance center)
4. **Validate implementation** through security assessments and audits

---

## 🔗 **Next Steps**

### **Continue Your Security Journey**
Now that you understand the shared responsibility model, you're ready to dive deeper into specific security concepts:

**Next Recommended Reading:**
- [Task 2.2: Security, Governance, and Compliance Concepts](./Task%20Statement%202.2-Understand%20AWS%20Cloud%20security,%20governance,%20and%20compliance%20concepts.md)

### **Additional Resources**
- [AWS Shared Responsibility Model Official Documentation](https://aws.amazon.com/compliance/shared-responsibility-model/)
- [AWS Security Best Practices Whitepapers](https://aws.amazon.com/architecture/security-identity-compliance/)
- [Domain 2 Overview](./README.md)

---

## ⭐ **Key Takeaways**

### **Remember These Core Concepts:**
1. **Security is shared** - AWS secures the cloud infrastructure, you secure your data and applications
2. **Your responsibility varies** by service type - more managed services mean less work for you
3. **You're ALWAYS responsible** for IAM, data encryption, and network configuration
4. **When in doubt, assume it's your responsibility** - it's better to over-secure than under-secure

### **Success Mantra:**
*"AWS gives me secure building blocks, but I must build securely with them."*

**Think of AWS like a high-security apartment building** - they provide excellent infrastructure security, but you still need to lock your door, secure your valuables, and be careful who you give your keys to.

---

*🎯 **Learning Checkpoint**: You now understand the fundamental principle of cloud security - shared responsibility. This knowledge forms the foundation for all other security concepts in AWS. Make sure you're comfortable with this model before moving on to specific security services and features.*

*🔒 **Security Mindset**: Always ask "Is this my responsibility or AWS's?" when encountering any security-related task. This simple question will guide you toward the right solution and the right team to contact for help.*
