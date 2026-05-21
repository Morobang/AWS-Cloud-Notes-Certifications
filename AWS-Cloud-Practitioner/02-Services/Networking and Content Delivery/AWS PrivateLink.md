# AWS PrivateLink

## What you'll learn
- What PrivateLink is and the problem it solves
- VPC Endpoints: Interface vs Gateway
- PrivateLink vs VPC Peering
- Key exam facts

---

## What is AWS PrivateLink?

AWS PrivateLink provides **private connectivity between VPCs and AWS services, third-party services, or your own services** — without routing traffic over the public internet. Traffic stays on the AWS network.

PrivateLink powers VPC Endpoints — allowing you to access AWS services (S3, DynamoDB, etc.) privately from within your VPC without needing an Internet Gateway or NAT Gateway.

---

## The problem PrivateLink solves

By default, when an EC2 instance in a private subnet calls an AWS service like S3 or DynamoDB, the traffic must go through a NAT Gateway to reach the service's public endpoint on the internet — even though both are on AWS infrastructure.

PrivateLink (via VPC Endpoints) lets that traffic stay entirely within the AWS network, with no internet exposure.

---

## Two types of VPC Endpoints

**Interface Endpoint** — Creates an Elastic Network Interface (ENI) with a private IP address in your VPC. Traffic to the target service routes to this private IP — stays on the AWS network. Supports most AWS services and third-party services (via AWS Marketplace). Powered by PrivateLink.

**Gateway Endpoint** — A route table target (not an ENI). Currently supports only Amazon S3 and DynamoDB. Free to use. Traffic routes through the gateway without leaving AWS.

---

## PrivateLink for custom services

You can also use PrivateLink to expose your own service (running on EC2, ECS, or an NLB) to consumers in other VPCs or other AWS accounts — privately, without VPC Peering or internet exposure.

Example: A SaaS provider exposes their service via PrivateLink. Customers create an Interface Endpoint in their own VPC and access the service over the AWS network without complex network configuration.

---

## PrivateLink vs VPC Peering

| | AWS PrivateLink | VPC Peering |
|-|----------------|-------------|
| Purpose | Private access to a specific service/endpoint | Full network connectivity between VPCs |
| Scope | Uni-directional (consumer → service) | Bi-directional |
| IP range overlap | Not affected | CIDRs must not overlap |
| Best for | Accessing AWS services or exposing a service | General VPC-to-VPC connectivity |

---

## When to use it

**Access AWS services privately** — EC2 instances in a private subnet access S3 or DynamoDB without internet exposure.

**Compliance** — Some regulations require that data never traverse the public internet — PrivateLink ensures this for AWS API calls.

**SaaS service consumption** — Access a SaaS provider's service via PrivateLink without opening network routes between VPCs.

---

## Exam focus

- PrivateLink = **private connectivity** to AWS services and custom services — no internet
- **Interface Endpoint** = ENI in your VPC — private IP for accessing services (most services)
- **Gateway Endpoint** = route table entry for S3 and DynamoDB (free)
- Use when exam describes: "access S3 without internet gateway," "private access to AWS service," "keep traffic on AWS network"

---

## Practice questions

**Q1.** A company has EC2 instances in a private subnet that need to access Amazon S3. They don't want any traffic leaving the AWS network or requiring a NAT Gateway. Which solution enables private S3 access?

A) VPC Peering  
B) AWS Transit Gateway  
C) A VPC Gateway Endpoint for S3  
D) AWS Direct Connect

**Answer: C** — A VPC Gateway Endpoint for S3 routes S3 traffic through the endpoint via the route table, keeping it on the AWS network. No NAT Gateway or Internet Gateway needed. VPC Peering connects two VPCs. Transit Gateway connects multiple VPCs. Direct Connect is for on-premises to AWS connectivity.
