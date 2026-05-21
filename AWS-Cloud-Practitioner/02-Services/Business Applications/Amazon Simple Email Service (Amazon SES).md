# Amazon Simple Email Service (Amazon SES)

## What you'll learn
- What Amazon SES is and what types of emails it sends
- How SES differs from SNS email notifications
- Key SES features and concepts
- Key exam facts

---

## What is Amazon SES?

Amazon Simple Email Service (SES) is a **cloud-based email sending service** for applications. It is designed for high-volume, programmatic email delivery — sending transactional emails (order confirmations, password resets), marketing emails (newsletters, promotions), and bulk communications at scale.

SES handles the complexities of email delivery: bounce handling, deliverability optimization, ISP relationships, and compliance.

---

## Types of email SES handles

**Transactional email** — One-to-one emails triggered by user actions:
- Order confirmation after a purchase
- Password reset link
- Account verification email
- Shipping notification

**Marketing email** — One-to-many promotional communications:
- Weekly newsletters
- Promotional campaigns
- Product announcements

**Bulk email** — Large-scale campaigns sent to mailing lists

---

## Key features

- **High deliverability**: AWS manages IP reputation and relationships with major ISPs (Gmail, Outlook, Yahoo) to maximize inbox delivery rates
- **Sending statistics**: Delivery, bounce, and complaint rates per campaign
- **Bounce and complaint handling**: Automatically removes invalid addresses (hard bounces) and unsubscribers (complaints) from your list
- **DKIM and SPF support**: Email authentication to prevent spoofing
- **Templates**: Create reusable email templates with variable substitution
- **Suppression list management**: Maintains a list of addresses that should not receive email
- **Inbound email processing**: Can also receive and process incoming email, routing it to S3, Lambda, or SNS

---

## SES vs SNS for email

| | Amazon SES | Amazon SNS (email) |
|-|-----------|-------------------|
| Purpose | Application email delivery at scale | Simple alert notifications |
| Volume | Millions of emails per day | Low-volume alerts |
- | Formatting | HTML and plain text with full control | Plain text only |
| Bounce handling | Full bounce/complaint management | Basic |
| Use case | Transactional and marketing emails | CloudWatch alarms, simple alerts |

---

## When to use it

**Application-triggered emails** — Every time a user registers, your application calls SES to send a welcome email.

**E-commerce notifications** — Order confirmations, shipping updates, and return confirmations sent automatically from your order management system.

**Marketing campaigns** — Newsletter delivery to subscriber lists, managed with SES sending stats and list hygiene.

**Inbound email processing** — Receive emails at your domain, store them in S3, and trigger Lambda to process them automatically.

---

## Exam focus

- SES = **programmatic email sending** from applications — transactional and marketing emails
- High-volume, high-deliverability email service
- Handles **bounce and complaint management** automatically
- Supports **inbound email** routing to S3, Lambda, or SNS
- Key differentiator from SNS: SES is for formatted, application-driven email; SNS email is for simple operational alerts
- Use when the exam mentions: "send email from application," "order confirmation," "password reset email," "marketing newsletter," "email at scale"

---

## Practice questions

**Q1.** An e-commerce company needs to automatically send order confirmation emails to customers immediately after each purchase. The emails should include HTML formatting with product images and order details. Which AWS service is most appropriate?

A) Amazon SNS  
B) Amazon Connect  
C) Amazon SES  
D) Amazon Pinpoint

**Answer: C** — Amazon SES is designed for programmatic, high-volume email delivery from applications. It supports HTML-formatted emails with full deliverability management. SNS can send emails but only plain text, and is designed for operational notifications rather than customer-facing transactional emails. Connect is a contact center service.
