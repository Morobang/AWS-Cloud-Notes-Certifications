# AWS CodeBuild

## What you'll learn
- What CodeBuild does and where it fits in a CI/CD pipeline
- How a build project works
- CodeBuild vs self-managed build servers
- Key exam facts

---

## What is AWS CodeBuild?

AWS CodeBuild is a **fully managed build service** that compiles source code, runs unit tests, and produces deployable artifacts. You don't manage build servers — CodeBuild provisions, scales, and terminates build environments automatically for each build job.

CodeBuild is the "build stage" in a CI/CD pipeline.

---

## How CodeBuild works

1. You create a **build project** that points to your source code (CodeCommit, GitHub, S3, or Bitbucket)
2. You provide a **buildspec.yml** file — a YAML file that defines the build commands (install dependencies, compile, run tests, package artifacts)
3. CodeBuild launches a temporary build environment (a Docker container), runs the commands, produces output artifacts, and terminates the environment
4. Artifacts are typically stored in S3 for downstream deployment

---

## buildspec.yml structure

The buildspec.yml has phases:

- **install** — Install dependencies (npm install, pip install, etc.)
- **pre_build** — Login to registries, run pre-checks
- **build** — Compile code, run tests
- **post_build** — Package artifacts, push Docker images

---

## Key features

- **Managed service**: No build servers to provision, patch, or maintain
- **On-demand scaling**: Multiple builds run in parallel automatically
- **Pay per build minute**: You only pay for the time builds are running — no idle server costs
- **Pre-built environments**: Managed images for Java, Python, Node.js, Ruby, Go, Android, .NET, Docker
- **Custom environments**: Bring your own Docker image if you need a specific build environment
- **Integration**: Connects with CodePipeline, CloudWatch Logs, S3, ECR, and SNS

---

## CodeBuild vs self-managed build servers

| | AWS CodeBuild | Jenkins / Self-Managed |
|-|--------------|------------------------|
| Infrastructure | Fully managed by AWS | You manage servers |
| Scaling | Automatic | Manual or custom auto-scaling |
| Pricing | Per build minute | Always-on server costs |
| Maintenance | None | Plugins, updates, patches |
| Best for | AWS-native CI/CD | Existing Jenkins investment |

---

## When to use it

**New CI/CD pipelines on AWS** — Start with CodeBuild instead of setting up and maintaining a Jenkins server.

**Variable build loads** — If builds happen intermittently, pay-per-use pricing is more cost-efficient than a running build server.

**Containerized build environments** — Build and push Docker images to ECR as part of the build process.

---

## Exam focus

- CodeBuild = **managed build service** — compiles code and runs tests
- No servers to manage — AWS handles scaling and infrastructure
- Uses **buildspec.yml** to define build commands
- Produces artifacts stored in **S3**
- **Pay per build minute** — no idle costs
- Part of the **AWS CI/CD toolchain**: CodePipeline (orchestration) → CodeBuild (build) → CodeDeploy (deploy)

---

## Practice questions

**Q1.** A team wants to automatically compile their Java application and run unit tests every time code is pushed to their repository. They do not want to manage build servers. Which AWS service handles this?

A) AWS CodePipeline  
B) AWS CodeBuild  
C) AWS CodeDeploy  
D) AWS Elastic Beanstalk

**Answer: B** — CodeBuild is a managed build service that compiles code and runs tests without requiring you to manage servers. CodePipeline orchestrates the pipeline but does not build code itself. CodeDeploy handles deployment. Elastic Beanstalk is a PaaS that deploys applications but is not a build service.
