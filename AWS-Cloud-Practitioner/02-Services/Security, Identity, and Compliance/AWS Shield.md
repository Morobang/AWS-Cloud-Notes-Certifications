# AWS Shield

## What you'll learn
- What DDoS attacks are and why they matter
- AWS Shield Standard vs Advanced
- Shield integration with other services
- Key exam facts

---

## What is AWS Shield?

AWS Shield is a **managed Distributed Denial of Service (DDoS) protection service** that safeguards AWS applications from DDoS attacks. It comes in two tiers: Standard (free, always-on) and Advanced (paid, enhanced protection).

---

## What is a DDoS attack?

A DDoS attack floods a service with massive volumes of traffic from many sources simultaneously — overwhelming its capacity and making it unavailable to legitimate users.

Types of DDoS attacks Shield protects against:
- **Volumetric attacks** — Flood bandwidth with massive traffic (UDP floods, DNS amplification)
- **State-exhaustion attacks** — Exhaust connection tables (TCP SYN floods)
- **Application layer attacks** — Overwhelm application logic (HTTP floods, Slowloris)

---

## AWS Shield Standard

- **Free** — automatically enabled for all AWS customers
- Protects against most common, smaller-scale DDoS attacks
- Provides always-on network flow monitoring
- Automatically mitigates attacks at the network and transport layers (Layer 3 and 4)
- Protects: CloudFront, Route 53, Elastic Load Balancing, Global Accelerator

For the vast majority of AWS customers, Shield Standard is sufficient.

---

## AWS Shield Advanced

- **Paid** — $3,000/month per organization (covers all resources in the organization)
- Enhanced protection for:
  - EC2, Elastic Load Balancing, CloudFront, Route 53, Global Accelerator
  - Application layer (Layer 7) DDoS protection
- **AWS DDoS Response Team (DRT)** — 24/7 access to AWS security experts who can help during active attacks
- **Cost protection** — AWS reimburses scaling costs incurred as a direct result of a DDoS attack (e.g., Auto Scaling adding EC2 instances to absorb attack traffic)
- **DDoS event visibility** — Detailed attack diagnostics and near-real-time metrics in CloudWatch

---

## Shield + WAF

AWS Shield Advanced integrates with AWS WAF to provide Layer 7 application protection. WAF handles malicious HTTP requests; Shield handles volumetric attacks.

---

## Exam focus

- Shield = **DDoS protection** — Standard (free, automatic) and Advanced (paid, enhanced)
- **Shield Standard**: free, always on, protects against most common attacks
- **Shield Advanced**: paid, 24/7 DRT support, Layer 7 protection, cost reimbursement
- Use when exam describes: "protect against DDoS attacks," "mitigate volumetric attacks"
- Shield is always enabled at the Standard level — you don't have to do anything

---

## Practice questions

**Q1.** A company hosts a high-profile web application on AWS and wants protection against large-scale DDoS attacks, including application layer attacks, with access to AWS security experts who can help during an active attack. Which AWS Shield tier provides this?

A) AWS Shield Standard  
B) AWS Shield Advanced  
C) AWS WAF  
D) AWS Firewall Manager

**Answer: B** — Shield Advanced provides enhanced Layer 7 protection, 24/7 access to the AWS DDoS Response Team, and cost protection for scaling during attacks. Shield Standard (free) protects against common volumetric attacks but doesn't provide Layer 7 protection or DRT access. WAF blocks malicious HTTP requests but is not a DDoS mitigation service by itself. Firewall Manager centrally manages WAF and Shield policies.
