# Lab 5: Explore VPC Basics

**Time:** ~15 minutes  
**Cost:** Free (exploring the default VPC, no new resources)  
**Concepts covered:** VPC, Subnets, Security Groups, Route Tables, Internet Gateway

---

## What You'll Explore

Every AWS account comes with a **Default VPC** in each Region. This lab explores that VPC to understand networking without creating anything new.

---

## Step 1: View the Default VPC

1. Search for **"VPC"** in the console → click VPC
2. In the left menu, click **"Your VPCs"**
3. You'll see the **Default VPC** — note its IPv4 CIDR block (`172.31.0.0/16`)

A CIDR block defines the range of IP addresses available in this network. `/16` means 65,536 possible IP addresses.

---

## Step 2: View the Subnets

1. Left menu → **"Subnets"**
2. You'll see multiple subnets — one per Availability Zone in the Region
3. Note each subnet's CIDR (e.g., `172.31.0.0/20`, `172.31.16.0/20`)
4. Note the **Availability Zone** column — each subnet lives in a specific AZ

**Public vs Private subnets:**
- A subnet is **public** if it has a route to an Internet Gateway
- Default VPC subnets are all public (any EC2 launched here gets a public IP)

---

## Step 3: View the Internet Gateway

1. Left menu → **"Internet gateways"**
2. You'll see one attached to your default VPC
3. An Internet Gateway is what connects your VPC to the public internet

Without an Internet Gateway, resources in a VPC can't communicate with the internet.

---

## Step 4: View Route Tables

1. Left menu → **"Route tables"**
2. Click the route table associated with the default VPC
3. Click the **"Routes"** tab
4. You'll see two routes:
   - `172.31.0.0/16 → local` — traffic within the VPC stays in the VPC
   - `0.0.0.0/0 → igw-xxxxx` — all other traffic goes to the Internet Gateway

This route table is what makes the subnets "public" — the `0.0.0.0/0` route sends traffic to the internet.

---

## Step 5: View Security Groups

1. Left menu → **"Security groups"**
2. You'll see a **default security group** and any ones you created in earlier labs
3. Click the default security group → view **Inbound rules** and **Outbound rules**

Default security group:
- Inbound: allows all traffic from the same security group
- Outbound: allows all traffic to anywhere

---

## Conceptual Understanding Check

Answer these in your head:

1. What would happen if you deleted the Internet Gateway? (Answer: resources lose internet access)
2. If a subnet has no route to the Internet Gateway, what is it called? (Answer: private subnet)
3. If a Security Group has no inbound rules, can anything reach the EC2 instance? (Answer: no — all inbound blocked by default)
4. Security Groups are stateful — what does that mean? (Answer: if you allow inbound traffic on port 80, the response is automatically allowed back out)

---

## Clean Up

Nothing to clean up — you only explored existing resources.

---

## What You Learned

- VPC = your isolated private network in AWS
- Subnets divide a VPC into smaller networks, each in a specific AZ
- Internet Gateway = the door to/from the public internet
- Route Tables = traffic directions (which traffic goes where)
- Security Groups = per-instance firewalls (stateful, allow-only)

---

## Exam Relevance

Networking questions often appear on the CCP exam:
- Know: VPC → Subnets → EC2 instances
- Know: Public subnet (has IGW route) vs Private subnet (no IGW route)
- Know: Security Group (stateful, instance-level) vs NACL (stateless, subnet-level)
- Know: Internet Gateway enables public internet access; NAT Gateway enables private subnet outbound
