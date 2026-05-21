# Lab 3: Launch Your First EC2 Instance

**Time:** ~20 minutes  
**Cost:** Free Tier (t2.micro/t3.micro for 750 hours/month in first 12 months)  
**Concepts covered:** EC2, AMI, Instance Types, Key Pairs, Security Groups, SSH

---

## What You'll Build

A Linux EC2 instance running in the cloud that you can connect to via browser (EC2 Instance Connect — no SSH client needed).

---

## Step 1: Navigate to EC2

1. Sign in to AWS console as your IAM admin user
2. Make sure you're in **us-east-1 (N. Virginia)** — check the Region selector top-right
3. Search for **"EC2"** → click it → click **"Launch instance"**

---

## Step 2: Configure the Instance

Fill in the launch wizard:

| Setting | Value |
|---|---|
| Name | `my-first-ec2` |
| Application and OS | **Amazon Linux 2023 AMI** (free tier eligible) |
| Instance type | **t2.micro** (free tier eligible) |
| Key pair | Click **"Create new key pair"**, name it `my-first-key`, type RSA, format `.pem`, download it |
| Security group | Create new: allow **SSH** from "My IP" |

Leave everything else as default.

3. Click **"Launch instance"**

---

## Step 3: Connect to the Instance

1. Go to **EC2 → Instances**
2. Wait until your instance shows **"Running"** and **"2/2 checks passed"** (takes ~2 minutes)
3. Select the instance → click **"Connect"**
4. Choose **"EC2 Instance Connect"** tab → click **"Connect"**

A browser-based terminal opens — you're now logged in to your Linux server in the cloud!

---

## Step 4: Explore the Instance

Run these commands in the terminal:

```bash
# See what OS you're running
cat /etc/os-release

# See how much CPU and RAM the instance has
nproc  # number of CPUs
free -h  # RAM

# See the instance metadata (unique to cloud)
curl http://169.254.169.254/latest/meta-data/instance-id
curl http://169.254.169.254/latest/meta-data/instance-type
curl http://169.254.169.254/latest/meta-data/placement/availability-zone
```

The metadata URL (`169.254.169.254`) is a special AWS endpoint that gives instances information about themselves.

---

## Step 5: Understand What You Just Did

In the console, look at your running instance:
- **AMI** — the "image" (like a snapshot) the OS was installed from
- **Instance type** — the hardware configuration (t2.micro = 1 vCPU, 1GB RAM)
- **Availability Zone** — which specific data center it's running in
- **Security Group** — the firewall rules

Go to **EC2 → Security Groups** — see the inbound rule you created (SSH from your IP).

---

## IMPORTANT: Clean Up (Required — avoids charges after Free Tier)

1. Select your instance in the EC2 console
2. Click **Instance state** → **Terminate instance**
3. Confirm termination

Terminated instances stop running and stop incurring charges. The instance is permanently deleted.

---

## What You Learned

- EC2 instances are virtual machines running in AWS data centers
- AMIs are templates (like a pre-installed OS image) for launching instances
- Instance types define CPU/RAM/network — t2.micro is the Free Tier option
- Security Groups control which traffic can reach your instance
- Instances run in a specific AZ within a Region

---

## Exam Relevance

- EC2 is the most-tested service in Domain 3
- Know: AMIs, instance types, key pairs, security groups, On-Demand vs Reserved vs Spot pricing
- Know: EC2 is IaaS — you manage the OS and apps, AWS manages the hardware
