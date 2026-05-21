# AWS Certificate Manager (ACM)

## What you'll learn
- What ACM does and why SSL/TLS certificates matter
- Free certificates vs imported certificates
- How ACM integrates with AWS services
- Key exam facts

---

## What is AWS Certificate Manager?

AWS Certificate Manager (ACM) is a service that **provisions, manages, and renews SSL/TLS certificates** for use with AWS services. It eliminates the manual, error-prone process of purchasing, deploying, and renewing certificates.

---

## What SSL/TLS certificates do

SSL/TLS certificates enable HTTPS — encrypting data in transit between a client (browser) and a server. They also prove the identity of the server (that you're really connecting to `example.com` and not an impostor).

Without a certificate, web browsers show "Not Secure" warnings; users may not trust the site.

---

## Key features

**Free public certificates** — ACM provides free public SSL/TLS certificates for use with supported AWS services. No cost, no renewal process — ACM auto-renews certificates before they expire.

**Automatic renewal** — ACM automatically renews managed certificates before expiration (no more scrambling when a cert expires and breaks your site).

**Private certificates** — ACM Private CA allows you to create a private certificate authority for internal certificates (internal services, microservices, IoT devices).

**Imported certificates** — Import certificates from external CAs (DigiCert, Comodo, etc.) into ACM to manage them centrally.

---

## Where ACM certificates are used

ACM certificates can be deployed on:
- **Elastic Load Balancers** — HTTPS listener on ALB/NLB
- **Amazon CloudFront** — Custom HTTPS domain for your distribution
- **Amazon API Gateway** — Custom domain with HTTPS
- **AWS Elastic Beanstalk** — HTTPS for deployed applications

ACM certificates **cannot be used on EC2 instances directly** — they can only be used with integrated AWS services.

---

## ACM vs self-managed certificates

| | AWS ACM | Self-Managed |
|-|---------|-------------|
| Cost | Free (for public certs with AWS services) | Certificate purchase fee |
| Renewal | Automatic | Manual (risk of expiration) |
| Deployment | One-click with ALB, CloudFront, API Gateway | Manual installation and rotation |
| Private CAs | ACM Private CA | On-premises PKI |

---

## Exam focus

- ACM = **free, managed SSL/TLS certificates** for HTTPS on AWS services
- **Automatic renewal** — certificates never expire unexpectedly
- Works with: **ELB, CloudFront, API Gateway**
- Cannot be installed on EC2 directly (only on integrated services)
- Use when exam describes: "HTTPS certificate for CloudFront," "SSL/TLS for load balancer," "manage certificates"

---

## Practice questions

**Q1.** A company wants to enable HTTPS on their Application Load Balancer using a custom domain. They want the SSL certificate to be automatically renewed without manual intervention. Which AWS service provides this at no additional cost?

A) AWS KMS  
B) AWS Secrets Manager  
C) AWS Certificate Manager (ACM)  
D) AWS CloudHSM

**Answer: C** — ACM provides free public SSL/TLS certificates that automatically renew before expiration. ACM certificates can be deployed directly on ALBs. KMS manages encryption keys, not certificates. Secrets Manager stores secrets. CloudHSM provides dedicated hardware security modules for cryptographic operations — not certificate provisioning.
