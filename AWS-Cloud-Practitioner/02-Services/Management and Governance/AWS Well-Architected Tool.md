# AWS Well-Architected Tool

## What you'll learn
- What the Well-Architected Framework is
- What the Well-Architected Tool does
- The six pillars
- Key exam facts

---

## What is the AWS Well-Architected Framework?

The AWS Well-Architected Framework is a set of **best practices for designing and operating cloud workloads** on AWS. It provides guidance across six pillars that represent key areas of architectural quality.

The framework is documented in whitepapers and guides, but the **Well-Architected Tool** brings it into your AWS account as an interactive review.

---

## The six pillars

| Pillar | Core question | Focus |
|--------|--------------|-------|
| Operational Excellence | How do you run and monitor systems? | Automate operations, improve over time |
| Security | How do you protect data and systems? | IAM, encryption, incident response |
| Reliability | How do you recover from failures? | Redundancy, backup, fault tolerance |
| Performance Efficiency | How do you use resources efficiently? | Right instance types, managed services |
| Cost Optimization | How do you avoid unnecessary costs? | Right-sizing, Reserved Instances, unused resources |
| Sustainability | How do you minimize environmental impact? | Maximize utilization, use managed services |

---

## What is the Well-Architected Tool?

The AWS Well-Architected Tool is a **free service in the AWS console** that guides you through a structured review of your workload against the six pillars. It asks you questions about your architecture and generates a report identifying:

- **High-risk issues (HRIs)**: Significant gaps that need immediate attention
- **Medium-risk issues (MRIs)**: Areas that should be improved
- **Best practice improvements**: Recommendations for each pillar

---

## How a review works

1. Define a workload (name, description, the workload being reviewed)
2. Answer a set of questions for each pillar (e.g., "How do you protect your data at rest?")
3. The tool generates a report showing your answers, identified risks, and improvement recommendations
4. Track improvement milestones over time

---

## Well-Architected Tool vs Trusted Advisor

| | Well-Architected Tool | Trusted Advisor |
|-|----------------------|-----------------|
| How it works | You answer questions in a guided review | Automated scan of your account |
| Output | Architectural guidance and risk report | Specific resource-level findings |
| Focus | Architectural decisions and practices | Actual resource configurations |
| Who it's for | Architects reviewing a workload design | Operations teams monitoring resources |

---

## When to use it

**Architecture review before launch** — Review a new workload design against the six pillars before production deployment.

**Post-incident analysis** — After a reliability or security event, use the tool to identify architectural gaps that contributed.

**Ongoing improvement** — Regularly review workloads to track improvement over time.

---

## Exam focus

- Well-Architected Tool = **guided architecture review** against the six pillars
- Six pillars: **Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, Sustainability**
- Free tool in the AWS console
- Generates reports identifying high-risk and medium-risk issues
- Use when exam describes: "review architecture against best practices," "identify architectural risks," "six pillars"

---

## Practice questions

**Q1.** An architect wants to formally evaluate whether their new application architecture follows AWS best practices across areas including security, reliability, and cost optimization. Which AWS service provides a guided review framework for this purpose?

A) AWS Trusted Advisor  
B) AWS Config  
C) AWS Well-Architected Tool  
D) AWS Compute Optimizer

**Answer: C** — The Well-Architected Tool guides architects through a structured review of their workload against the six pillars of the AWS Well-Architected Framework, identifying architectural risks and improvement recommendations. Trusted Advisor scans actual resource configurations for specific issues. Config checks resource compliance against rules. Compute Optimizer focuses on right-sizing compute resources.
