# AWS Service Catalog

## What you'll learn
- What Service Catalog is and what problem it solves
- Products, portfolios, and constraints
- Key use cases
- Key exam facts

---

## What is AWS Service Catalog?

AWS Service Catalog is a service that **allows organizations to create and manage catalogs of approved AWS products** that end users can self-service deploy. It gives IT teams control over what can be deployed while giving developers the speed to provision resources without waiting for IT approval on every request.

---

## The problem it solves

Without Service Catalog, teams face a choice:
- **Too restrictive**: Lock down IAM so developers can't provision anything without IT involvement — slow
- **Too permissive**: Give developers broad access — they provision unapproved, insecure, or expensive resources

Service Catalog provides a middle path: IT defines what's allowed (approved configurations, pre-approved templates), and developers self-service deploy from the approved catalog — fast but within guardrails.

---

## Key concepts

**Product** — A CloudFormation template packaged as a deployable product. Examples: "Approved Linux EC2 instance," "Standard S3 bucket with encryption," "Three-tier web application." Products have versions.

**Portfolio** — A collection of products. Portfolios are shared with specific users, groups, or roles — giving them access to only the products in their portfolio.

**Constraints** — Rules applied to products in a portfolio:
- **Launch constraints**: Which IAM role to use when launching — users can deploy without needing direct IAM permissions to the underlying services
- **Template constraints**: Restrict which parameters a user can change (e.g., lock the instance type to approved sizes)
- **Notification constraints**: Send SNS notifications when products are launched

**Provisioned product** — An instance of a deployed product. Service Catalog tracks all provisioned products centrally.

---

## Service Catalog vs CloudFormation

| | Service Catalog | CloudFormation |
|-|----------------|----------------|
| Who uses it | End users (developers, business users) | Infrastructure engineers |
| Access control | Portfolios and constraints limit choices | Direct IAM access to CloudFormation |
| Product versions | Managed — IT controls updates | Template files in Git |
| Best for | Governed self-service deployment | Direct IaC by engineers |

Service Catalog uses CloudFormation templates as the underlying provisioning mechanism — it adds the governance layer on top.

---

## When to use it

**Approved product catalog** — An enterprise wants developers to be able to deploy databases, but only from pre-approved, security-reviewed configurations (encryption enabled, private subnets only).

**Cost control** — Restrict which instance types are available for launch, preventing expensive choices.

**Compliance** — Ensure all deployed resources meet security and compliance baselines built into the templates.

---

## Exam focus

- Service Catalog = **approved product catalog** — self-service deployment within IT-defined guardrails
- **Products** = CloudFormation templates packaged for end users
- **Portfolios** = collections of products shared with users
- **Constraints** = restrict what users can configure
- Use when exam describes: "self-service provisioning with guardrails," "approved resource catalog," "end users deploy without direct IAM permissions to AWS services"

---

## Practice questions

**Q1.** A large enterprise wants developers to be able to deploy databases and web servers on-demand without waiting for IT approvals, but only using pre-approved, security-compliant configurations. Which AWS service provides this governed self-service capability?

A) AWS CloudFormation  
B) AWS Control Tower  
C) AWS Service Catalog  
D) AWS Organizations

**Answer: C** — Service Catalog provides a self-service portal where developers can deploy from pre-approved products (CloudFormation templates vetted by IT). Constraints limit the configurations available, ensuring compliance. CloudFormation is the underlying tool but doesn't provide the governance layer. Control Tower sets up multi-account structure. Organizations manages account hierarchy and billing.
