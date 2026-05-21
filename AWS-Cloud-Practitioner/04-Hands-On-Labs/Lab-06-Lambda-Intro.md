# Lab 6: Try AWS Lambda

**Time:** ~15 minutes  
**Cost:** Free (Lambda free tier: 1M requests/month — always free)  
**Concepts covered:** Lambda, Serverless, Event-driven architecture, Functions as a Service

---

## What You'll Build

A Lambda function that takes a name as input and returns a personalized greeting — your first piece of serverless code running in the cloud.

---

## Step 1: Create a Lambda Function

1. Search for **"Lambda"** in the console → click Lambda
2. Click **"Create function"**
3. Choose **"Author from scratch"**
4. Configure:
   - Function name: `hello-from-lambda`
   - Runtime: **Python 3.12** (or latest Python available)
   - Architecture: x86_64
5. Click **"Create function"**

---

## Step 2: Write Your Function Code

In the Code editor, replace all the existing code with this:

```python
import json

def lambda_handler(event, context):
    # Get the name from the event (or use a default)
    name = event.get('name', 'World')
    
    message = f"Hello, {name}! You're running serverless on AWS Lambda."
    
    return {
        'statusCode': 200,
        'body': json.dumps({
            'message': message,
            'input_received': event
        })
    }
```

Click **"Deploy"** to save your changes.

---

## Step 3: Test the Function

1. Click the **"Test"** tab
2. Click **"Create new test event"**
3. Event name: `TestEvent`
4. Replace the JSON with:
```json
{
  "name": "AWS Learner"
}
```
5. Click **"Save"**
6. Click **"Test"**

You should see a green "Execution result: succeeded" with your message.

---

## Step 4: Understand What Just Happened

Look at the execution result details:
- **Duration** — how long your code ran (usually under 5ms)
- **Billed duration** — what you'd pay for (rounded up to nearest 1ms)
- **Memory used** — RAM consumed
- **Init duration** — the "cold start" — Lambda had to initialize your function the first time

With 1 million free requests per month, you'd need to call this function 1,000,000 times to hit the free tier limit. That's basically free for learning.

---

## Step 5: Explore the Configuration

Click the **"Configuration"** tab:
- **General configuration**: memory limit, timeout (max 15 minutes), environment variables
- **Triggers**: what events can invoke this function (API Gateway, S3 events, DynamoDB streams, scheduled events, etc.)
- **Permissions**: the IAM role Lambda uses — right now it only has permission to write CloudWatch logs

---

## Step 6: View the Logs

1. Run the test a few more times
2. Click **"Monitor"** tab → **"View CloudWatch logs"**
3. Click the latest log stream to see the execution logs

Every Lambda invocation is automatically logged to CloudWatch Logs. No setup needed.

---

## Clean Up

Lambda functions at rest don't cost anything — you're only charged for invocations. But if you want to clean up:

1. In Lambda → Functions → select `hello-from-lambda` → Actions → Delete

---

## What You Learned

- Lambda is serverless — no servers to provision, patch, or scale
- You upload code, Lambda handles the execution
- Event-driven: something triggers the function (in this case, a test event; in production, maybe an API call or file upload)
- Pay per execution (duration × memory) — cost is essentially zero for low usage
- Lambda auto-scales: 1 request or 1 million requests — same code, AWS handles it

---

## Exam Relevance

Lambda is a major exam topic for serverless:
- Know: Lambda is FaaS (Function as a Service) — you don't manage servers
- Know: Max execution timeout = 15 minutes
- Know: Triggers: API Gateway, S3 events, DynamoDB Streams, EventBridge, SQS
- Know: Lambda vs EC2 — Lambda for short event-driven tasks, EC2 for long-running/stateful workloads
