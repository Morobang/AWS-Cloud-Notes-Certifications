# Lab 1: Set Up Billing Alerts

**Time:** ~10 minutes  
**Cost:** Free  
**Concepts covered:** AWS Budgets, Cost Management, Free Tier monitoring

---

## Why This Lab First?

Before doing anything else in AWS, protect yourself from surprise bills. This lab takes 10 minutes and could save you from a nasty surprise at end of month.

---

## What You'll Build

An AWS Budget that sends you an email alert when:
1. Your monthly costs exceed $5
2. Your Free Tier usage reaches 80%

---

## Step 1: Enable Billing Alerts

1. Sign in to the AWS Console as your **root user** (the email/password you used to create the account)
2. Click your account name in the top-right corner → **Account**
3. Scroll down to **Billing preferences**
4. Enable **"Receive Free Tier usage alerts"** — enter your email address
5. Enable **"Receive billing alerts"**
6. Click **Save preferences**

---

## Step 2: Create a Cost Budget

1. In the search bar, type **"Budgets"** → click **AWS Budgets**
2. Click **Create budget**
3. Choose **"Use a template (simplified)"**
4. Select **"Monthly cost budget"**
5. Set:
   - Budget name: `Monthly-Cost-Alert`
   - Budgeted amount: `$5`
   - Email recipients: your email address
6. Click **Create budget**

You now have an alert that fires if you spend more than $5 in a month.

---

## Step 3: Check Your Free Tier Usage

1. Click your account name → **Billing and Cost Management**
2. In the left menu, click **Free Tier**
3. You'll see a table of every Free Tier service, how much you've used, and your limit

Bookmark this page — check it weekly while learning.

---

## What You Learned

- AWS Budgets: set spending limits and get email alerts
- Free Tier tracking: monitor usage to avoid unexpected charges
- Best practice: always set up billing protection before experimenting

---

## Clean Up

Nothing to clean up — Budgets themselves are free.

---

## Exam Relevance

- AWS Budgets → Domain 4: Billing, Pricing & Support
- Understand the difference between AWS Budgets (alerts/thresholds) vs Cost Explorer (analysis of past spend)
