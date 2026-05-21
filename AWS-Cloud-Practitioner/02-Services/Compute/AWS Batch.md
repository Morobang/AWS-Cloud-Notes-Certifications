# AWS Batch

## What you'll learn
- What AWS Batch is and what batch computing means
- How AWS Batch differs from Lambda and EC2
- Key use cases
- Exam facts

---

## What is AWS Batch?

AWS Batch is a **managed batch computing service** that automatically provisions the right amount of compute capacity to run batch jobs efficiently. You submit jobs to a queue; Batch dynamically provisions EC2 (or Spot) instances, runs the jobs, and terminates the instances when done.

**Batch computing** means processing a large number of discrete jobs (as opposed to continuous workloads) — for example, processing 10,000 image files, running 500 simulation jobs, or generating 1 million reports.

---

## How it works

1. You define a **job definition** — the Docker container image, required CPU and memory, and any environment variables
2. You create a **job queue** — jobs submitted here wait for capacity
3. AWS Batch creates a **compute environment** (EC2 or Fargate) sized to run the queued jobs
4. Jobs run in Docker containers; when complete, compute resources are released
5. Job results are stored in S3, RDS, or wherever your container writes output

---

## AWS Batch vs Lambda vs EC2

| | AWS Batch | AWS Lambda | EC2 |
|-|-----------|-----------|-----|
| Workload type | Batch jobs (queued, discrete) | Event-driven functions | Continuous or long-running |
| Max duration | No limit (hours or days) | 15 minutes | No limit |
| Infrastructure | Managed by Batch | Managed by Lambda | You manage |
| Scaling | Automatic based on queue | Automatic | Manual or Auto Scaling |
| Use case | Bulk data processing | Triggered processing | General-purpose servers |

---

## When to use it

**Scientific computing** — Run 10,000 protein folding simulations overnight. Batch provisions a large cluster, processes all jobs, then terminates everything.

**Financial modeling** — End-of-day batch processing of all trades across multiple instruments.

**Image/video processing** — Process all uploaded images for a day in a batch job during off-peak hours.

**Data transformation** — Transform raw S3 data files into processed output files.

---

## Exam focus

- Batch = **managed batch computing** — you submit jobs to a queue, Batch handles the compute
- Runs workloads in **Docker containers** on EC2 or Fargate
- Automatically provisions and scales compute based on the job queue depth
- Can use **Spot Instances** to reduce cost (great for batch workloads that can tolerate interruption)
- No time limit (unlike Lambda's 15-minute limit)
- Use when exam describes: "batch processing," "job queue," "process large numbers of jobs," "scheduled compute jobs"

---

## Practice questions

**Q1.** A genomics company needs to process 50,000 genome sequencing files each night. Each job takes 20 minutes and requires 8 vCPUs and 32 GB of memory. Which service should orchestrate this workload?

A) AWS Lambda  
B) AWS Batch  
C) Amazon EC2 Auto Scaling  
D) Amazon ECS

**Answer: B** — AWS Batch is designed for exactly this: managing a queue of batch jobs, automatically provisioning appropriate compute (in this case, high-memory EC2 instances or Fargate tasks), processing them, and releasing resources when done. Lambda has a 15-minute limit and can't meet the 20-minute runtime requirement. Manual EC2 Auto Scaling lacks job queue management. ECS could work but requires more manual orchestration.
