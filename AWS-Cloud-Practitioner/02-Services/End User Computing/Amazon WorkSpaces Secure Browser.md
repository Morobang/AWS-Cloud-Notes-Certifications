# Amazon WorkSpaces Secure Browser

## What you'll learn
- What WorkSpaces Secure Browser is and the problem it solves
- How it differs from WorkSpaces and AppStream 2.0
- Key use cases
- Key exam facts

---

## What is Amazon WorkSpaces Secure Browser?

Amazon WorkSpaces Secure Browser is a **fully managed, cloud-based browser service** that lets users access internal web applications and websites without exposing corporate data to their local device.

The browser session runs in AWS. The user's local device sees only a display stream — no data is downloaded to or stored on the end device.

---

## The problem it solves

Organizations often need to give employees, contractors, or third parties access to internal web applications — HR portals, internal dashboards, business tools. But allowing access from unmanaged personal devices creates risk:

- Data could be downloaded and saved locally
- Browser extensions could leak data
- Malware on the personal device could capture credentials

WorkSpaces Secure Browser isolates the browser session in AWS, preventing data from reaching the local device.

---

## How it works

1. The administrator creates a portal with WorkSpaces Secure Browser
2. Users open their local browser and navigate to the portal URL
3. A browser session launches in AWS — the user interacts with websites through this cloud browser
4. Web content, downloads, and data stay in the AWS session — not on the user's device
5. When the session ends, no data remains on the local machine

---

## Key features

- **No client software required**: Users only need their existing browser — nothing to install
- **Data isolation**: Downloads and browsing activity stay in the cloud session
- **Managed service**: AWS handles provisioning, scaling, and infrastructure
- **Session recording**: Optional recording of browser sessions for compliance
- **SAML/IdP integration**: Authenticate users through existing identity providers

---

## WorkSpaces Secure Browser vs WorkSpaces vs AppStream 2.0

| | WorkSpaces Secure Browser | Amazon WorkSpaces | AppStream 2.0 |
|-|--------------------------|-------------------|---------------|
| What's delivered | Isolated browser session | Full persistent desktop | Specific application |
| Purpose | Secure web access from unmanaged devices | Replace physical desktop | Stream a Windows app |
| Client required | No (uses existing browser) | Yes (WorkSpaces client) | No (browser) |
| Data persistence | Session-based | Persistent | Session-based |
| Best for | Contractors accessing internal web apps | Remote employees needing a full PC | Delivering one specific app |

---

## When to use it

**Third-party contractors** — Give contractors access to internal web portals without providing them a managed corporate device.

**BYOD environments** — Allow employees to use personal devices to access corporate web applications without risking data leakage.

**Compliance requirements** — Ensure sensitive web application data never reaches unmanaged endpoints.

---

## Exam focus

- WorkSpaces Secure Browser = **managed cloud browser** — isolates browser sessions from end-user devices
- No software installed on the user's device
- Data stays in AWS — nothing is downloaded to the local machine
- Key differentiator: Secure Browser is for **web access only**; WorkSpaces is a full desktop; AppStream is for specific applications

---

## Practice questions

**Q1.** A financial services firm needs to give third-party auditors temporary access to internal web-based reporting dashboards. The auditors use personal laptops that are not managed by the firm. No data should be stored on the auditors' devices. Which AWS service best addresses this?

A) Amazon WorkSpaces  
B) Amazon AppStream 2.0  
C) Amazon WorkSpaces Secure Browser  
D) Amazon EC2

**Answer: C** — WorkSpaces Secure Browser provides an isolated cloud browser session for accessing web applications. The auditors use their existing browser — no client installation — and no data is stored on their devices. WorkSpaces would provide a full desktop (more than needed). AppStream is for streaming specific non-browser applications. EC2 would require management overhead and doesn't provide data isolation from the end device.
