# AWS Amplify

## What you'll learn
- What AWS Amplify is and what it provides
- Amplify Hosting vs Amplify Studio
- Key use cases for web and mobile developers
- Key exam facts

---

## What is AWS Amplify?

AWS Amplify is a **set of tools and services for building full-stack web and mobile applications**. It allows frontend and mobile developers to quickly connect their apps to AWS backend services (authentication, APIs, storage, databases) without deep AWS expertise.

Amplify handles the heavy lifting: deploying the frontend, setting up CI/CD, provisioning backend infrastructure, and generating client-side code to connect to AWS services.

---

## Two main components

**Amplify Hosting** — A fully managed hosting service for web applications (similar to Netlify or Vercel). Connect your GitHub/GitLab/Bitbucket repository; Amplify automatically builds and deploys your app on every commit. Supports static sites and server-side rendered (SSR) applications.

**Amplify Studio** — A visual development environment for building UI components and backend resources. Non-developers can configure data models, authentication, and storage through a visual interface.

---

## Backend services Amplify connects to

| Feature | AWS service behind it |
|---------|----------------------|
| Authentication | Amazon Cognito |
| REST or GraphQL API | API Gateway or AWS AppSync |
| Storage (files) | Amazon S3 |
| Database | Amazon DynamoDB |
| Functions | AWS Lambda |

Amplify generates the client-side configuration and SDK code so your app can connect to these services with minimal setup.

---

## Key features

- **CI/CD hosting**: Every git push triggers a build and deploys to a hosted URL — no manual deployment steps
- **Preview deployments**: Every pull request gets its own preview URL
- **Custom domains**: Connect a custom domain with SSL certificate provisioning
- **Auth integration**: Add sign-up, sign-in, and social login (Google, Facebook) in minutes using Cognito
- **Multi-framework support**: React, Angular, Vue, Next.js, iOS, Android, Flutter

---

## When to use it

**Startups and MVPs** — Get a web or mobile app with auth, API, and storage deployed in hours instead of days.

**Static and JAMstack sites** — Host a React or Vue app with automatic deployments from Git.

**Mobile app backends** — Provide authentication, API, push notifications, and storage to a mobile app without building custom backend infrastructure.

---

## Exam focus

- Amplify = **full-stack web and mobile app platform** — hosting, CI/CD, auth, API, storage
- **Amplify Hosting** = deploy and host web apps directly from Git — similar to Netlify/Vercel
- Uses Cognito for auth, AppSync/API Gateway for APIs, S3 for storage, DynamoDB for data
- Key differentiator from Elastic Beanstalk: Amplify is for frontend/mobile apps; Beanstalk is for server-side web applications

---

## Practice questions

**Q1.** A startup team is building a React web application. They want automatic deployment to a hosted URL every time they push to GitHub, built-in user authentication, and an API connected to DynamoDB. Which AWS service is designed for this use case?

A) AWS Elastic Beanstalk  
B) Amazon EC2  
C) AWS Amplify  
D) Amazon S3 Static Website

**Answer: C** — Amplify provides CI/CD hosting from Git, authentication via Cognito, and an API layer connected to DynamoDB — all with minimal configuration. Elastic Beanstalk is for server-side applications. EC2 requires manual management. S3 static hosting can serve a React app but doesn't provide auth, API, or CI/CD out of the box.
