# AWS CloudFormation

## What you'll learn
- What Infrastructure as Code (IaC) is
- What CloudFormation does and how templates work
- Key benefits: repeatability, drift detection
- Key exam facts

---

## What is AWS CloudFormation?

AWS CloudFormation is an **Infrastructure as Code (IaC) service** that lets you define your AWS infrastructure in a template file (JSON or YAML), and CloudFormation provisions and manages all the resources described in the template.

Instead of clicking through the console to create a VPC, then subnets, then EC2 instances, then security groups — you write a template that describes all of these, and CloudFormation creates them in the right order.

---

## Infrastructure as Code

The core idea: treat infrastructure configuration the same way you treat application code — version it, review it, test it, and deploy it consistently.

**Benefits**:
- **Repeatability**: The same template creates identical environments in dev, staging, and production
- **Automation**: No manual console clicks — deploy entire stacks with one command
- **Version control**: Store templates in Git — roll back to previous infrastructure state
- **Documentation**: The template is a living document of what you deployed

---

## Key concepts

**Template** — A YAML or JSON file describing the AWS resources to create. A template can define EC2 instances, VPCs, RDS databases, IAM roles, Lambda functions, and more.

**Stack** — A single deployment of a template. When you create a stack, CloudFormation provisions all resources in the template. When you delete the stack, CloudFormation deletes all those resources.

**StackSets** — Deploy the same stack across multiple AWS accounts and Regions with a single operation. Used by large organizations to apply standard infrastructure across many accounts.

**Drift detection** — CloudFormation can compare a deployed stack against its template to detect if resources were manually changed outside of CloudFormation (drift). Useful for compliance.

**Change sets** — Preview what changes CloudFormation will make before applying a template update. Like a diff — shows which resources will be added, modified, or deleted.

---

## CloudFormation vs Terraform

CloudFormation is AWS-native. Terraform (by HashiCorp) is a third-party IaC tool that supports multi-cloud deployments. Both serve the same purpose. For the exam, only CloudFormation is in scope.

---

## When to use it

**Consistent environment deployment** — Deploy identical dev/staging/prod environments from the same template.

**Disaster recovery** — Rebuild an entire infrastructure stack in a different Region from a template in minutes.

**Compliance and governance** — Ensure all infrastructure is created through approved templates, preventing ad-hoc console changes.

---

## Exam focus

- CloudFormation = **Infrastructure as Code** — define resources in a YAML/JSON template; CloudFormation creates them
- **Stack** = one deployment of a template
- **StackSets** = deploy across multiple accounts/Regions
- **Drift detection** = compare live resources to template to find manual changes
- Use when exam describes: "deploy infrastructure consistently," "repeatability," "IaC," "template-based provisioning"

---

## Practice questions

**Q1.** A company wants to ensure their production environment is identical to their staging environment and can be recreated from scratch in a new Region if needed. Which AWS service enables this through Infrastructure as Code?

A) AWS Systems Manager  
B) AWS CloudFormation  
C) AWS Config  
D) Amazon CloudWatch

**Answer: B** — CloudFormation templates define all infrastructure resources. The same template can be deployed in any Region to create identical environments, and it can recreate the entire stack if needed. Systems Manager handles operational tasks (patching, parameter store). Config tracks configuration changes but doesn't provision infrastructure. CloudWatch monitors metrics and logs.
