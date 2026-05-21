# Lab 7: Set Up a CloudWatch Alarm

**Time:** ~15 minutes  
**Cost:** Free (10 CloudWatch alarms always free)  
**Concepts covered:** CloudWatch, Metrics, Alarms, Monitoring, Observability

---

## What You'll Build

A CloudWatch alarm that sends an email when EC2 CPU usage exceeds 80% — the kind of alert that wakes a DevOps engineer at 3am in real production environments.

---

## Step 1: Launch a Temporary EC2 Instance

You need an EC2 instance to monitor. Quick launch:

1. Go to EC2 → **Launch instance**
2. Name: `cloudwatch-lab`
3. AMI: Amazon Linux 2023 (free tier)
4. Instance type: t2.micro
5. Key pair: select existing or "Proceed without keypair" (for this lab)
6. Click **Launch instance**

---

## Step 2: Set Up SNS Notification

CloudWatch alerts need somewhere to send notifications. Create an SNS topic first:

1. Search for **"SNS"** → click Simple Notification Service
2. Click **"Create topic"**
3. Type: **Standard**
4. Name: `my-alerts`
5. Click **Create topic**
6. In the topic, click **Create subscription**
7. Protocol: **Email**
8. Endpoint: your email address
9. Click **Create subscription**
10. Check your email → click the confirmation link in the AWS email

Your email is now subscribed to receive alerts.

---

## Step 3: Create a CloudWatch Alarm

1. Search for **"CloudWatch"** → click CloudWatch
2. Left menu → **Alarms** → **All alarms** → **Create alarm**
3. Click **Select metric**
4. Click **EC2** → **Per-Instance Metrics**
5. Find your `cloudwatch-lab` instance → select **CPUUtilization** → click **Select metric**

Configure the alarm:
- Period: **1 minute**
- Threshold type: **Static**
- Condition: **Greater than** `80`
- Datapoints to alarm: `1 out of 1`
- Click **Next**

6. Notification:
   - Alarm state trigger: **In alarm**
   - Send notification to: select your `my-alerts` SNS topic
   - Click **Next**

7. Alarm name: `High-CPU-Alert`
8. Click **Create alarm**

---

## Step 4: Explore CloudWatch Metrics

While you're in CloudWatch:

1. Left menu → **Metrics** → **All metrics**
2. Click **EC2** → **Per-Instance Metrics**
3. Search for your instance ID and view available metrics:
   - CPUUtilization
   - NetworkIn / NetworkOut
   - DiskReadOps / DiskWriteOps
4. Select a few metrics and click the **Graph** tab — you can see the real-time graphs

This is how SREs monitor production systems.

---

## Step 5: View CloudWatch Logs

1. Left menu → **Logs** → **Log groups**
2. If you completed Lab 6 (Lambda), you'll see `/aws/lambda/hello-from-lambda` here
3. Click into it to see log streams and individual function execution logs

CloudWatch Logs automatically collects logs from Lambda, EC2 agents, ECS, and other services.

---

## Clean Up

1. **Delete the EC2 instance**: EC2 → Instances → select instance → Terminate
2. **Delete the alarm**: CloudWatch → Alarms → select alarm → Actions → Delete
3. **Delete the SNS topic**: SNS → Topics → select → Delete (optional)

---

## What You Learned

- CloudWatch collects metrics automatically from all AWS services
- You can create alarms that trigger on threshold breaches
- Alarms notify via SNS → which can send email, SMS, or trigger Lambda
- CloudWatch Logs aggregates application and service logs centrally
- This is the "Observability" layer of every production AWS environment

---

## Exam Relevance

- CloudWatch = monitoring, metrics, logs, alarms
- Remember: CloudWatch ≠ CloudTrail
  - CloudWatch = HOW IS IT performing (metrics/logs)
  - CloudTrail = WHO DID WHAT (API audit trail)
- Common exam pattern: "How do you get notified when X happens?" → CloudWatch Alarm + SNS
