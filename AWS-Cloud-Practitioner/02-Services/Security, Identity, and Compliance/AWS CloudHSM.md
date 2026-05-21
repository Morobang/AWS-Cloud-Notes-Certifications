# AWS CloudHSM

## What you'll learn
- What an HSM is and what CloudHSM provides
- CloudHSM vs KMS
- Key use cases
- Key exam facts

---

## What is AWS CloudHSM?

AWS CloudHSM is a **cloud-based hardware security module (HSM)** that provides dedicated, tamper-resistant hardware for generating and using encryption keys. Unlike KMS (where key operations happen on shared AWS infrastructure), CloudHSM gives you a dedicated physical HSM device in your VPC that only you control.

---

## What is an HSM?

A Hardware Security Module (HSM) is a physical device designed specifically for cryptographic operations. It:
- Generates true random numbers for key generation
- Stores keys in tamper-resistant hardware
- Performs cryptographic operations (encryption, signing) inside the hardware
- Erases keys if tampered with
- Is certified to FIPS 140-2 Level 3 (CloudHSM) — the highest hardware security level

---

## CloudHSM vs AWS KMS

| | AWS CloudHSM | AWS KMS |
|-|-------------|---------|
| Hardware | Dedicated HSM — yours alone | Shared AWS HSMs |
| Control | You manage the HSM; AWS manages physical security | AWS fully manages |
| FIPS level | FIPS 140-2 Level 3 | FIPS 140-2 Level 2 |
| Compliance | Strict regulatory requirements | Most use cases |
| Cost | Higher (dedicated hardware) | Lower (shared service) |
| Encryption keys | Customer-managed, AWS never sees keys | AWS manages keys on your behalf (for AWS Managed Keys) |

**Key distinction**: With KMS Customer Managed Keys, AWS manages the key material on HSMs on your behalf. With CloudHSM, you control the HSM and AWS never has access to your key material.

---

## When CloudHSM is required

Some regulatory requirements or use cases need the control and assurance of dedicated hardware:
- **FIPS 140-2 Level 3** validation specifically (not Level 2)
- Contracts requiring cryptographic operations to happen on customer-controlled hardware
- Key escrow requirements where the customer must be the sole custodian
- PKI (Public Key Infrastructure) where you operate your own Certificate Authority

---

## When to use it

**Strict compliance mandates** — Certain financial, government, or healthcare regulations require FIPS Level 3 dedicated hardware.

**Customer-managed key material** — When you need to ensure that AWS has absolutely no access to your encryption keys.

**SSL/TLS offloading** — Use the HSM to store and use SSL certificates for web servers.

---

## Exam focus

- CloudHSM = **dedicated hardware security module** for cryptographic operations
- **FIPS 140-2 Level 3** — highest hardware certification (KMS is Level 2)
- AWS manages the hardware but **you manage the keys** — AWS cannot access key material
- More expensive than KMS; used when strict compliance or customer-controlled keys are required
- Use when exam describes: "dedicated HSM," "FIPS Level 3," "customer controls all key material," "AWS cannot access encryption keys"

---

## Practice questions

**Q1.** A government agency needs to encrypt sensitive data using encryption keys stored on dedicated hardware that they fully control. They have a specific requirement for FIPS 140-2 Level 3 compliance. Which AWS service meets this requirement?

A) AWS KMS  
B) AWS Secrets Manager  
C) AWS CloudHSM  
D) AWS Certificate Manager

**Answer: C** — CloudHSM provides dedicated FIPS 140-2 Level 3 certified hardware in your VPC. You fully control the HSM and the key material — AWS never has access. KMS uses shared hardware (FIPS Level 2) and AWS manages the HSM infrastructure. Secrets Manager stores secrets, not encryption key material. ACM manages SSL/TLS certificates.
