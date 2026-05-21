# Amazon WorkSpaces

## What you'll learn
- What Amazon WorkSpaces is and what problem it solves
- WorkSpaces vs physical desktops
- Pricing models
- Key exam facts

---

## What is Amazon WorkSpaces?

Amazon WorkSpaces is a **fully managed virtual cloud desktop service**. Each WorkSpace is a persistent cloud-based Windows or Linux desktop that a user accesses remotely using a client application or web browser.

Instead of buying and maintaining physical laptops or PCs for every employee, you provision WorkSpaces in the cloud. The desktop, its applications, and its data all live in AWS.

---

## How WorkSpaces works

1. An administrator provisions a WorkSpace for a user — choosing an OS (Windows or Linux), a hardware bundle (vCPUs, RAM, storage), and applications
2. The user installs the WorkSpaces client on any device (Windows, Mac, iPad, Android, Chromebook, or Linux) or accesses it via a browser
3. The user logs in and sees their persistent desktop — exactly as they left it last time
4. Data stays in AWS — not on the user's physical device

---

## Key features

- **Persistent desktop**: User data, settings, and installed applications persist between sessions
- **Managed**: AWS handles the underlying hardware, patching of the WorkSpaces OS, and infrastructure
- **Multi-device**: Access from laptops, tablets, thin clients, or browsers
- **Active Directory integration**: Integrates with AWS Directory Service or Microsoft Active Directory for user authentication
- **Bring your own licenses**: Use existing Windows licenses to reduce cost
- **Two pricing models**: Monthly (always-on) or hourly (pay for usage)

---

## WorkSpaces pricing models

**Monthly pricing** — Pay a flat monthly fee per WorkSpace regardless of hours used. Best for users who work full-time every day.

**Hourly pricing** — Pay per hour the WorkSpace is actually running (plus a small monthly fee for storage). Best for part-time workers, contractors, or seasonal employees.

---

## WorkSpaces vs physical desktops

| | Amazon WorkSpaces | Physical Desktops |
|-|------------------|-------------------|
| Hardware procurement | Not required | Significant upfront cost |
| Maintenance | AWS manages hardware; admin manages software | IT team maintains hardware and OS |
| Data location | Stays in AWS | On the physical device (theft risk) |
| Access | Any device, any location | Tied to physical location |
| Scaling | Provision in minutes | Order, ship, configure |

---

## When to use it

**Remote and distributed workforce** — Give remote employees a full Windows or Linux desktop accessible from any device or location.

**Replace aging hardware** — Instead of refreshing physical PCs on a 3–4 year cycle, provision WorkSpaces on demand.

**Contractors and temporary staff** — Quickly provision a workspace and deprovision it when the contract ends — no physical hardware to recover.

**Regulated industries** — Keep all data in AWS rather than on employee devices — reduces data loss and theft risk.

---

## Exam focus

- WorkSpaces = **virtual cloud desktops** — persistent Windows or Linux desktop per user
- Managed service — AWS handles underlying infrastructure
- Users access from any device using the WorkSpaces client or browser
- **Monthly or hourly** pricing — use hourly for part-time or variable users
- Key differentiator from AppStream 2.0: WorkSpaces = full desktop; AppStream = specific application streaming

---

## Practice questions

**Q1.** A company is transitioning to a fully remote workforce. They want to provide employees with a full Windows desktop environment accessible from any device, while keeping all corporate data off personal devices. Which AWS service best meets this requirement?

A) Amazon AppStream 2.0  
B) Amazon WorkSpaces  
C) Amazon EC2 with Remote Desktop  
D) AWS Elastic Beanstalk

**Answer: B** — WorkSpaces provides persistent cloud-based Windows desktops accessible from any device. All data stays in AWS. AppStream 2.0 streams individual applications, not full desktops. EC2 with Remote Desktop is possible but requires manual provisioning, patching, and management. Elastic Beanstalk is a PaaS for web applications.
