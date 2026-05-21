# Developer Tools — Category Overview

AWS Developer Tools help teams build, test, deploy, and observe applications. For the Cloud Practitioner exam, focus on understanding what each tool does at a high level and which stage of the software delivery lifecycle it addresses.

---

## Services in this category

| Service | Purpose | Key concept |
|---------|---------|-------------|
| AWS CLI | Command-line interface to control AWS services | Scripting and automation alternative to the console |
| AWS CodeBuild | Managed build service — compiles code and runs tests | Replaces Jenkins/CircleCI for build stages |
| AWS CodePipeline | CI/CD orchestration — automates source → build → deploy | Connects source, build, and deploy stages into one pipeline |
| AWS X-Ray | Distributed tracing — visualize request flow across services | Identify latency bottlenecks and errors in microservices |

---

## How they fit together

A typical CI/CD pipeline on AWS:

1. Developer pushes code to CodeCommit (or GitHub)
2. CodePipeline detects the change and starts the pipeline
3. CodeBuild compiles the code and runs tests
4. CodePipeline deploys the artifact to Elastic Beanstalk, ECS, or Lambda
5. X-Ray traces live requests in production to identify issues

---

## Exam selection guide

- "Automate build and test on every commit" → **CodeBuild**
- "Automate the entire release pipeline from source to production" → **CodePipeline**
- "Identify which microservice is causing latency in a distributed application" → **X-Ray**
- "Script AWS tasks without clicking the console" → **AWS CLI**
