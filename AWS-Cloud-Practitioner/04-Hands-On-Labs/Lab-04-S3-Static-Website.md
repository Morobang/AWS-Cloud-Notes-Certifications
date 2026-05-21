# Lab 4: Create an S3 Bucket and Host a Static Website

**Time:** ~20 minutes  
**Cost:** Free Tier (5GB S3 Standard storage for 12 months)  
**Concepts covered:** S3, Object Storage, Bucket Policies, Static Website Hosting, Public Access

---

## What You'll Build

A public S3 bucket hosting a simple static HTML website, accessible via a URL — like a personal webpage hosted entirely in S3 with no servers.

---

## Step 1: Create an S3 Bucket

1. Search for **"S3"** in the AWS console → click S3
2. Click **"Create bucket"**
3. Configure:
   - Bucket name: `my-first-website-<your-name>-2024` (must be globally unique)
   - Region: `us-east-1`
   - **Uncheck** "Block all public access" (you need public access for a website)
   - Check the acknowledgment box that appears
4. Leave everything else as default
5. Click **"Create bucket"**

---

## Step 2: Upload Your Website Files

Create this simple HTML file on your computer and save it as `index.html`:

```html
<!DOCTYPE html>
<html>
<head>
  <title>My AWS S3 Website</title>
  <style>
    body { font-family: Arial; text-align: center; padding: 50px; background: #f0f0f0; }
    h1 { color: #FF9900; }
  </style>
</head>
<body>
  <h1>Hello from Amazon S3!</h1>
  <p>This static website is hosted entirely in Amazon S3.</p>
  <p>No servers. No EC2. Just object storage.</p>
</body>
</html>
```

Upload it:
1. Click your bucket name → click **"Upload"**
2. Click **"Add files"** → select your `index.html`
3. Click **"Upload"**

---

## Step 3: Enable Static Website Hosting

1. In your bucket, click the **"Properties"** tab
2. Scroll to **"Static website hosting"** → click **"Edit"**
3. Set:
   - Enable: **Enabled**
   - Hosting type: **Host a static website**
   - Index document: `index.html`
4. Click **Save changes**
5. Scroll back down to Static website hosting — copy the **Bucket website endpoint** URL

---

## Step 4: Make the Objects Public

The bucket is configured for public access, but individual objects still need a policy.

1. Click the **"Permissions"** tab of your bucket
2. Scroll to **"Bucket policy"** → click **"Edit"**
3. Paste this policy (replace `YOUR-BUCKET-NAME` with your actual bucket name):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/*"
    }
  ]
}
```

4. Click **Save changes**

---

## Step 5: Visit Your Website

Open the Bucket website endpoint URL you copied in Step 3 in your browser.

You should see your HTML page — a live website hosted on S3!

---

## Explore S3 Concepts

While you're in the bucket:

1. **Storage Classes** — click your uploaded file → Properties → see the default storage class (Standard)
2. **Versioning** — go to Properties → Version → Enable versioning, then re-upload `index.html` with a change. Click the file → Versions tab to see both versions
3. **Lifecycle rules** — Properties → Lifecycle rules: this is where you'd auto-move old objects to cheaper storage tiers

---

## Clean Up

1. Delete all objects in the bucket first (select all → Delete)
2. Then delete the bucket itself (Actions → Delete bucket)

S3 charges for storage — even small amounts — so always clean up after labs.

---

## What You Learned

- S3 stores objects (files) in buckets — no hierarchy, no file system
- Bucket names must be globally unique across all of AWS
- Public access requires both: (1) disabling "Block Public Access" AND (2) a bucket policy
- Static website hosting turns an S3 bucket into a web server — no EC2 needed
- S3 is used for: backup, archival, data lakes, static websites, software distribution

---

## Exam Relevance

- S3 is a major exam topic — expect questions on:
- Storage classes (Standard, IA, Glacier, Deep Archive)
- Use cases (backups, static sites, data lakes)
- Versioning and lifecycle policies
- S3 as a component in serverless architectures
