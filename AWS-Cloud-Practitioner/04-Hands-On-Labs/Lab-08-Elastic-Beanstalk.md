# Lab 8: Deploy with Elastic Beanstalk

**Time:** ~30 minutes  
**Cost:** Free Tier eligible (EC2 t2.micro behind the scenes)  
**Concepts covered:** Elastic Beanstalk, PaaS, Auto Scaling, Load Balancing, Managed Deployment

---

## What You'll Build

Deploy a Python web application to Elastic Beanstalk — AWS automatically provisions EC2, Load Balancer, Auto Scaling, and everything else. You just provide the code.

---

## Step 1: Create Your Application Code

Create a folder on your computer called `my-beanstalk-app`. Inside it, create a file called `application.py`:

```python
from flask import Flask

application = Flask(__name__)

@application.route('/')
def home():
    return '''
    <html>
    <head><title>Beanstalk App</title></head>
    <body style="font-family: Arial; text-align: center; padding: 50px; background: #fff8f0;">
        <h1 style="color: #FF9900;">Running on Elastic Beanstalk!</h1>
        <p>AWS automatically provisioned EC2, Load Balancer, and Auto Scaling for this app.</p>
        <p>You just provided the code.</p>
    </body>
    </html>
    '''

if __name__ == '__main__':
    application.run()
```

Create a file called `requirements.txt`:
```
flask==3.0.0
```

Zip both files into `my-app.zip` (select both files → right-click → Compress/Zip).

---

## Step 2: Create an Elastic Beanstalk Environment

1. Search for **"Elastic Beanstalk"** → click it
2. Click **"Create application"**
3. Configure:
   - Application name: `my-first-app`
   - Platform: **Python**
   - Platform branch: Python 3.11 (or latest)
   - Application code: **Upload your code** → upload `my-app.zip`
4. Click **"Next"**

5. Service access:
   - Create a new service role (name it `aws-elasticbeanstalk-service-role`)
   - EC2 key pair: skip (optional for this lab)
   - EC2 instance profile: Create a new one or use default
6. Click **Next** through remaining steps, keeping defaults
7. Click **Submit**

Beanstalk now creates your environment — this takes 5-10 minutes as it provisions EC2, configures the load balancer, etc. Watch the events stream.

---

## Step 3: Visit Your Application

Once the environment shows **"OK"** (green health indicator):

1. Click the URL shown at the top of the environment dashboard
2. Your Flask application is live!

---

## Step 4: Explore What Beanstalk Created

Click around the Beanstalk environment dashboard:

- **Health**: Overall environment health (Green/Yellow/Red)
- **Monitoring**: CPU, requests, latency graphs (all automatically collected)
- **Configuration**: Instance type, auto scaling settings, environment variables, load balancer
- **Logs**: Request logs from your application

Now go check what was actually created:
1. Go to EC2 → Instances — see the EC2 instance Beanstalk created
2. Go to EC2 → Load Balancers — see the Application Load Balancer
3. Go to EC2 → Auto Scaling Groups — see the Auto Scaling configuration

**Beanstalk created all of this from your zip file.** This is PaaS — Platform as a Service.

---

## Step 5: Update the Application

Change the text in `application.py` (update the `<h1>` tag to say something else), re-zip the files, then:

1. Go to Beanstalk → your environment → click **Upload and deploy**
2. Upload the new zip → click **Deploy**

Beanstalk does a rolling deployment — updates instances one at a time to avoid downtime.

---

## Clean Up (Important — EC2 costs money!)

1. Go to Elastic Beanstalk → your environment → **Actions** → **Terminate environment**
2. Wait for the environment to terminate (this also removes the EC2 and Load Balancer)
3. Then go to Elastic Beanstalk → Applications → select your application → **Actions** → **Delete application**

---

## What You Learned

- Elastic Beanstalk = PaaS — you give code, AWS manages the infrastructure
- Behind the scenes: EC2, ELB, Auto Scaling, CloudWatch are all provisioned automatically
- You still have full access to those underlying resources if needed
- Deployment is simple: upload a zip, click deploy
- Rolling deployments prevent downtime during updates

---

## Exam Relevance

- Know: Beanstalk is PaaS — you manage the APPLICATION, AWS manages the platform
- Know: IaaS (EC2) vs PaaS (Beanstalk) vs SaaS (Gmail/Office365)
- Common exam scenario: "developer wants to deploy quickly without managing servers" → Elastic Beanstalk
- Beanstalk supports: Python, Java, Node.js, PHP, Ruby, Go, .NET, Docker
