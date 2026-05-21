# Amazon Lightsail

## What you'll learn
- What Lightsail is and who it's designed for
- What Lightsail includes and how it's priced
- When to use Lightsail vs EC2
- Key exam facts

---

## What is Amazon Lightsail?

Amazon Lightsail is a **simplified virtual private server (VPS) service** designed for developers, small businesses, and individuals who want a straightforward, low-cost way to launch servers and applications without the complexity of EC2.

Lightsail bundles compute, storage, and networking into a single **fixed monthly price** — making it easy to predict costs without understanding AWS's detailed pricing model.

---

## What Lightsail includes

A Lightsail instance comes with:
- A virtual server (pre-configured compute)
- SSD-based storage
- A static IP address
- DNS management
- A fixed amount of data transfer (outbound bandwidth)

All for a predictable flat monthly fee starting at a few dollars per month.

---

## Pre-configured application stacks

Lightsail provides one-click launch for common application stacks:

| Stack | Use |
|-------|-----|
| WordPress | Blog or CMS |
| Joomla / Drupal | Content management |
| LAMP stack | PHP web applications |
| Node.js | JavaScript backend |
| MEAN stack | MongoDB + Express + Angular + Node |
| Django | Python web framework |
| Nginx | Web server |
| cPanel | Web hosting control panel |

---

## Lightsail vs EC2

| | Amazon Lightsail | Amazon EC2 |
|-|----------------|------------|
| Pricing | Fixed monthly fee | Pay-per-second, complex pricing dimensions |
| Complexity | Simple — one-click launch | Full control, many configuration options |
| Flexibility | Limited instance types and options | Hundreds of instance types, all AWS features |
| Target user | Individuals, small businesses, beginners | Enterprises, developers needing full control |
| Scalability | Limited | Unlimited with Auto Scaling |

---

## When to use it

**Personal websites and blogs** — A small business launches a WordPress site for a few dollars per month.

**Development and test environments** — Developers need a cheap, quick VPS for testing without worrying about AWS pricing complexity.

**Simple web applications** — Small applications that don't need the full power of EC2's configuration options.

**Students and beginners** — Learn cloud computing basics without dealing with EC2's complexity.

---

## Exam focus

- Lightsail = **simple VPS with fixed monthly pricing** — designed for beginners and simple use cases
- Bundles compute, storage, networking, and a static IP in one predictable price
- **Not for enterprise workloads** — limited scalability and AWS service integration
- Key differentiator from EC2: Lightsail is simpler with predictable pricing; EC2 has full flexibility
- Use when the exam mentions: "simple website," "small business," "predictable low cost," "WordPress hosting," "no AWS expertise"

---

## Practice questions

**Q1.** A freelancer wants to host a WordPress blog on AWS. They have no AWS experience and want simple, predictable monthly pricing without needing to understand EC2, EBS, and VPC configuration. Which service is most appropriate?

A) Amazon EC2  
B) AWS Elastic Beanstalk  
C) Amazon Lightsail  
D) AWS Lambda

**Answer: C** — Lightsail provides a pre-configured WordPress instance with fixed monthly pricing. It bundles everything needed (compute, storage, networking) into a simple package designed for non-experts. EC2 provides more power but requires understanding many AWS concepts. Beanstalk is for deploying custom code, not for simple CMS hosting. Lambda is for event-driven functions.
