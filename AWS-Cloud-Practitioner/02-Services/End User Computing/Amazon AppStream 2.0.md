# Amazon AppStream 2.0

## What you'll learn
- What AppStream 2.0 does and how it differs from WorkSpaces
- Application streaming vs desktop streaming
- Common use cases
- Key exam facts

---

## What is Amazon AppStream 2.0?

Amazon AppStream 2.0 is a **fully managed application streaming service**. It delivers desktop applications to users through a web browser — the application runs on AWS infrastructure, not on the user's device.

Unlike WorkSpaces (which gives a full virtual desktop), AppStream 2.0 streams only the specific application. Users interact with the application in their browser without installing anything locally.

---

## How AppStream 2.0 works

1. You install your application on an AppStream 2.0 image (a preconfigured virtual machine)
2. AppStream 2.0 runs the application on its fleet of streaming instances
3. Users open a browser and access the application through a URL
4. Their inputs (mouse clicks, keystrokes) go to the cloud; the application's display streams back to their browser
5. No software is installed on the user's device

---

## Key features

- **Browser-based access**: Works on any device with a browser — no client installation required
- **Managed fleet**: AWS manages the underlying infrastructure; you manage the application image
- **Persistent storage**: User files can be stored in S3 or a home folder that persists between sessions
- **SAML 2.0 integration**: Connect to your existing identity provider for single sign-on
- **Scalable**: Automatically scales the number of streaming instances based on demand

---

## AppStream 2.0 vs Amazon WorkSpaces

| | AppStream 2.0 | Amazon WorkSpaces |
|-|--------------|-------------------|
| What's delivered | A specific application | A full desktop (Windows or Linux) |
| Persistence | Session-based (not persistent by default) | Persistent desktop per user |
| User experience | Application in a browser tab | Full desktop environment |
| Best for | Delivering one app to many users | Replacing physical desktops |
| Use case | CAD software, training platforms, internal tools | Remote employees needing a full PC |

---

## When to use it

**Deliver legacy Windows applications** — Stream a Windows application to users who run macOS or Linux devices, without requiring them to install Windows.

**Training and education** — Deliver a consistent, pre-configured application environment to students or trainees without managing individual machines.

**Contractors and temporary workers** — Give access to a specific business application without corporate device provisioning.

**SaaS-ify a desktop application** — Turn a traditional installed application into a browser-accessible service.

---

## Exam focus

- AppStream 2.0 = **application streaming** — specific apps delivered via browser, not a full desktop
- No software installed on user devices
- Applications run on **AWS infrastructure** — user's device only needs a browser
- Key differentiator from WorkSpaces: AppStream = one app streamed; WorkSpaces = full persistent desktop

---

## Practice questions

**Q1.** A software company wants to let customers try their Windows desktop application without downloading or installing it. Customers should be able to access the application from any browser. Which AWS service supports this?

A) Amazon WorkSpaces  
B) Amazon AppStream 2.0  
C) Amazon EC2  
D) AWS Elastic Beanstalk

**Answer: B** — AppStream 2.0 streams desktop applications to a web browser. The application runs on AWS and users interact with it through their browser — no installation required. WorkSpaces delivers a full desktop, not a single application. EC2 would require users to remote desktop into an instance. Elastic Beanstalk deploys web applications, not Windows desktop applications.
