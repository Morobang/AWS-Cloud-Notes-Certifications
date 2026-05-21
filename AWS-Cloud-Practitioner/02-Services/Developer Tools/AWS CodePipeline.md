# AWS CodePipeline

## What you'll learn
- What CodePipeline does and what problem it solves
- The stages in a typical pipeline
- How CodePipeline connects AWS developer tools
- Key exam facts

---

## What is AWS CodePipeline?

AWS CodePipeline is a **fully managed continuous delivery (CD) service** that automates the release process for applications. It orchestrates the steps from source code change to production deployment — connecting source, build, test, and deploy stages into a single automated workflow.

Without CodePipeline, teams manually trigger builds and deployments after each code change. CodePipeline automates this end-to-end.

---

## Pipeline stages

A pipeline is a sequence of stages. Each stage contains actions:

**Source stage** — Detects a code change in CodeCommit, GitHub, Bitbucket, or an S3 bucket. This triggers the pipeline.

**Build stage** — Passes the code to CodeBuild (or Jenkins) to compile and test.

**Test stage** (optional) — Run integration tests, load tests, or manual approvals before proceeding.

**Deploy stage** — Deploy the artifact to CodeDeploy, Elastic Beanstalk, ECS, Lambda, or CloudFormation.

---

## Example pipeline flow

```
Code push to GitHub
    → CodePipeline detects change
    → CodeBuild: compile + unit tests
    → (optional) Manual approval gate
    → CodeDeploy: deploy to EC2 fleet
```

---

## Key features

- **Fully managed**: No pipeline infrastructure to maintain
- **Visual pipeline editor**: See the state of each stage in the console
- **Parallel actions**: Run multiple actions in the same stage simultaneously
- **Manual approval**: Add a human approval step before deploying to production
- **Integration**: Works with CodeCommit, CodeBuild, CodeDeploy, Elastic Beanstalk, ECS, Lambda, CloudFormation, and third-party tools (GitHub, Jenkins, etc.)
- **Notifications**: Send SNS notifications on pipeline state changes

---

## CodePipeline vs CodeBuild

| | CodePipeline | CodeBuild |
|-|-------------|-----------|
| Role | Orchestrates the pipeline | Executes the build step |
| Scope | Source → Build → Deploy | Compile + test only |
| Runs code? | No — it connects services | Yes — runs build commands |
| Analogy | The conductor | One musician in the orchestra |

CodePipeline and CodeBuild are complementary — CodePipeline calls CodeBuild as one of its stages.

---

## When to use it

**Automated release process** — Trigger a build and deployment automatically every time code is pushed, eliminating manual steps.

**Multi-stage deployments** — Deploy to a staging environment first, require a manual approval, then promote to production.

**Multi-service pipelines** — Coordinate builds across multiple repositories or microservices.

---

## Exam focus

- CodePipeline = **CI/CD orchestration** — automates the full release pipeline
- Connects: **source** (CodeCommit/GitHub) → **build** (CodeBuild) → **deploy** (CodeDeploy/Beanstalk/ECS/Lambda)
- Managed service — no pipeline servers to maintain
- **Manual approval** actions can gate deployments to production
- Key differentiator from CodeBuild: CodePipeline orchestrates; CodeBuild builds

---

## Practice questions

**Q1.** A development team wants to automatically test and deploy their application to production every time code is merged to the main branch. They want a visual dashboard showing the status of each step. Which AWS service provides this orchestration?

A) AWS CodeBuild  
B) AWS CodeDeploy  
C) AWS CodePipeline  
D) AWS Elastic Beanstalk

**Answer: C** — CodePipeline orchestrates the entire CI/CD workflow from source detection through build and deploy, with a visual pipeline console showing the status of each stage. CodeBuild handles only the build step. CodeDeploy handles only deployment. Elastic Beanstalk deploys applications but does not orchestrate multi-stage pipelines.
