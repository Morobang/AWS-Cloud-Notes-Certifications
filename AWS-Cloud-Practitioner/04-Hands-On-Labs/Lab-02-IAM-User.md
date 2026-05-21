# Lab 2: Create Your First IAM User

**Time:** ~15 minutes  
**Cost:** Free (IAM is always free)  
**Concepts covered:** IAM Users, Groups, Policies, MFA, Principle of Least Privilege

---

## Why This Lab?

You should never use your root account for daily tasks. This lab sets you up with a proper IAM admin user — the correct way to work in AWS.

---

## What You'll Build

- An IAM User Group called `Administrators`
- An IAM User attached to that group with AdministratorAccess
- MFA enabled on the root account

---

## Part A: Secure the Root Account

1. Sign in as root user
2. Click your account name (top-right) → **Security credentials**
3. Under **Multi-factor authentication (MFA)**, click **Assign MFA device**
4. Choose **Authenticator app** (download Google Authenticator or Authy on your phone first)
5. Scan the QR code with your authenticator app
6. Enter two consecutive codes from the app to verify
7. Click **Add MFA**

Your root account is now protected with MFA. 

---

## Part B: Create an IAM Admin User

1. In the search bar, type **"IAM"** → click **IAM**
2. In the left menu, click **User groups** → **Create group**
3. Set:
   - Group name: `Administrators`
   - Attach policy: search for and select **`AdministratorAccess`**
4. Click **Create user group**

Now create the user:

5. In the left menu, click **Users** → **Create user**
6. Set:
   - User name: `admin` (or your name)
   - Enable: **"Provide user access to the AWS Management Console"**
   - Choose: **"I want to create an IAM user"**
   - Custom password: set a strong password
   - Uncheck "Users must create a new password at next sign-in" (for simplicity in the lab)
7. Click **Next**
8. Add user to group: select **Administrators**
9. Click **Next** → **Create user**
10. **Download the .csv file** with the login credentials — save it somewhere safe

---

## Part C: Sign In as the IAM User

1. Go to your AWS account's sign-in URL: `https://<account-id>.signin.aws.amazon.com/console`
   - You can find your account ID in the top-right corner of the console
2. Sign in with the IAM user credentials you just created
3. From now on, use this account — not root — for everything

---

## Part D: Explore IAM Policies

1. Go to IAM → **Policies**
2. Search for `AdministratorAccess` and click it
3. Look at the JSON — notice: `"Effect": "Allow", "Action": "*", "Resource": "*"`
4. Now search for `AmazonS3ReadOnlyAccess` — see how it restricts to specific S3 actions

This shows you how policies control what a user can and can't do.

---

## What You Learned

- IAM Users: individual identities with credentials
- IAM Groups: attach policies once, all group members inherit them
- IAM Policies: JSON documents defining allowed actions on resources
- MFA: second layer of security for root account
- Least privilege: give exactly the access needed — nothing more

---

## Clean Up

No cost to clean up. Keep the IAM admin user — you'll use it for all future labs.

---

## Exam Relevance

- IAM is heavily tested — expect 5-8 questions touching IAM concepts
- Know: root vs IAM users, roles vs users, policies, MFA, groups
- Remember: root = nuclear code. Secure it, don't use it daily.
