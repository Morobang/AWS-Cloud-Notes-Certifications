# End User Computing — Category Overview

AWS End User Computing services deliver desktops and applications to users through the cloud. Instead of provisioning physical hardware for every employee, organizations can stream desktops and applications securely from AWS.

---

## Services in this category

| Service | Purpose | Key concept |
|---------|---------|-------------|
| Amazon WorkSpaces | Fully managed virtual cloud desktops | Persistent desktop (Windows or Linux) per user |
| Amazon AppStream 2.0 | Stream applications to a browser | No desktop — just the application, streamed |
| Amazon WorkSpaces Secure Browser | Managed browser for accessing internal web apps | Isolates browser activity from the end device |

---

## Key distinction: desktop vs application streaming

**WorkSpaces** gives each user a full persistent Windows or Linux desktop in the cloud — like giving them a PC that lives in AWS.

**AppStream 2.0** streams only specific applications (not a full desktop) to a browser. The application runs on AWS; the user sees and interacts with it locally.

**WorkSpaces Secure Browser** provides a managed browser session for accessing internal web applications — isolating web browsing from the local device to prevent data loss.

---

## Exam selection guide

- "Replace physical desktops with cloud desktops" → **Amazon WorkSpaces**
- "Give users access to a specific application without installing it" → **Amazon AppStream 2.0**
- "Allow contractors to access internal web apps without installing software on their device" → **WorkSpaces Secure Browser**
- "Reduce hardware costs for a remote workforce" → **Amazon WorkSpaces**
