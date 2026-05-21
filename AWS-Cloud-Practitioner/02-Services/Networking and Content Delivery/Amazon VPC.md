# Amazon VPC (Virtual Private Cloud)

## What you'll learn
- What a VPC is and why it matters
- Subnets, routing, and internet connectivity
- Security: Security Groups and NACLs
- Key exam facts

---

## What is Amazon VPC?

Amazon Virtual Private Cloud (VPC) is a **logically isolated virtual network** in the AWS cloud. It lets you define your own IP address range, create subnets, configure route tables, and control inbound and outbound traffic — giving you full network control over your AWS resources.

Every EC2 instance, RDS database, and Lambda function runs within a VPC.

---

## Key concepts

**VPC** — A private network in AWS. You define a CIDR block (e.g., 10.0.0.0/16) that sets the IP address space.

**Subnets** — Subdivisions of a VPC within a specific Availability Zone. Two types:
- **Public subnet**: Has a route to an Internet Gateway — resources can be accessed from the internet
- **Private subnet**: No route to the internet — resources are isolated

**Internet Gateway (IGW)** — Attaches to a VPC and allows resources in public subnets to communicate with the internet. Without an IGW, nothing in the VPC can reach the internet.

**NAT Gateway** — Allows resources in *private* subnets to initiate outbound connections to the internet (e.g., to download software updates) without being directly reachable from the internet.

**Route tables** — Define where network traffic is directed. Each subnet is associated with a route table.

---

## Security in a VPC

**Security Groups** — Virtual firewalls for individual EC2 instances (or other resources):
- Stateful: if you allow inbound traffic, the response is automatically allowed outbound
- Operate at the instance level
- Allow rules only (no explicit deny rules)

**Network Access Control Lists (NACLs)** — Firewall at the subnet level:
- Stateless: inbound and outbound rules are evaluated independently
- Can have both allow and deny rules
- Applied to all resources in a subnet

| | Security Groups | NACLs |
|-|----------------|-------|
| Level | Instance | Subnet |
| Stateful? | Yes | No |
| Allow/Deny | Allow only | Both |

---

## VPC connectivity options

- **VPC Peering** — Connect two VPCs directly (same or different accounts/Regions) — private traffic between VPCs
- **Transit Gateway** — A hub connecting many VPCs and on-premises networks
- **VPN** — Encrypted tunnel over the internet to your VPC
- **Direct Connect** — Dedicated private link from on-premises to AWS

---

## Default VPC

AWS creates a default VPC in each Region for new accounts. It includes public subnets in each AZ with an Internet Gateway attached, allowing you to launch instances immediately without network configuration.

---

## Exam focus

- VPC = **your private isolated network in AWS**
- **Public subnet** = has internet gateway route; **Private subnet** = no internet access
- **Security Groups** = stateful, instance-level firewall (allow rules only)
- **NACLs** = stateless, subnet-level firewall (allow + deny)
- **NAT Gateway** = lets private subnet instances reach the internet outbound only
- Use when exam describes: "isolate resources," "private network," "control traffic between EC2 instances"

---

## Practice questions

**Q1.** A company has a web tier in a public subnet and a database tier in a private subnet. The database instances need to download security patches from the internet but should not be directly accessible from the internet. Which component allows the database instances to make outbound internet connections?

A) Internet Gateway  
B) VPC Peering  
C) NAT Gateway  
D) Security Group

**Answer: C** — A NAT Gateway in the public subnet allows instances in private subnets to initiate outbound connections to the internet without exposing them to inbound internet traffic. An Internet Gateway enables two-way internet connectivity (public subnets). VPC Peering connects two VPCs. Security Groups control traffic to specific instances but don't enable internet connectivity.
