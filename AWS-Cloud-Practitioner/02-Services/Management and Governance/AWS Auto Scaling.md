# AWS Auto Scaling

## What you'll learn
- What Auto Scaling is and why it matters
- EC2 Auto Scaling groups
- Scaling policies: reactive, scheduled, predictive
- Key exam facts

---

## What is AWS Auto Scaling?

AWS Auto Scaling automatically adjusts the number of compute resources in response to demand. It adds capacity when load increases (scale out) and removes it when load decreases (scale in) — keeping applications available while minimizing cost.

---

## EC2 Auto Scaling

The most common use is **EC2 Auto Scaling**, which manages groups of EC2 instances.

**Auto Scaling Group (ASG)** — A logical grouping of EC2 instances with:
- **Minimum capacity**: The floor — never fewer than this many instances
- **Desired capacity**: The target number of instances when load is normal
- **Maximum capacity**: The ceiling — never more than this many instances

The ASG adds instances when load increases and removes them when load drops, keeping instance count between the minimum and maximum.

---

## Scaling policies

**Dynamic scaling (reactive)** — Scale in response to CloudWatch alarms:
- "If average CPU > 70%, add 2 instances"
- "If average CPU < 30%, remove 1 instance"

**Scheduled scaling** — Scale based on time:
- "Add 5 instances every weekday at 8 AM"
- "Remove extra instances every weekday at 8 PM"
- Useful for predictable traffic patterns (business hours, known peak events)

**Predictive scaling** — ML analyzes historical traffic patterns and proactively adds capacity before predicted peaks. Reduces the lag between load increase and new capacity being available.

---

## What Auto Scaling can scale

| Service | Auto Scaling support |
|---------|---------------------|
| EC2 | Auto Scaling Groups |
| ECS tasks | Service Auto Scaling |
| DynamoDB | Read/write capacity units |
| Aurora | Read Replicas |
| Lambda | Concurrency (automatic, built-in) |

AWS Auto Scaling (the unified service) lets you set scaling policies for multiple services from a single interface.

---

## Auto Scaling and Elastic Load Balancing

Auto Scaling and ELB work together: when an ASG adds a new instance, ELB automatically starts routing traffic to it. When an instance is removed, ELB drains its connections and stops sending traffic before the instance terminates.

---

## Exam focus

- Auto Scaling = **automatically adjust capacity** based on demand or schedule
- **Minimum / desired / maximum** are the three capacity settings for an ASG
- Three policy types: **dynamic** (reactive), **scheduled** (time-based), **predictive** (ML-based)
- Auto Scaling + ELB = the standard pattern for **high availability and elasticity**
- Key differentiator: Auto Scaling scales the number of instances; ELB distributes traffic across them

---

## Practice questions

**Q1.** A web application experiences high traffic during business hours and minimal traffic overnight. The company wants to automatically scale EC2 instances up at 8 AM and down at 8 PM each weekday. Which Auto Scaling policy type handles this?

A) Dynamic scaling  
B) Predictive scaling  
C) Scheduled scaling  
D) Manual scaling

**Answer: C** — Scheduled scaling adjusts capacity on a defined schedule. Since the traffic pattern is known and predictable (business hours), scheduled scaling is the right choice. Dynamic scaling reacts to real-time metrics. Predictive scaling uses ML for patterns — also possible, but scheduled scaling is the direct answer for known, fixed schedules. Manual scaling requires human intervention.
