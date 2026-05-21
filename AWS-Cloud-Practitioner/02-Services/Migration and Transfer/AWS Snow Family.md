# AWS Snow Family

## What you'll learn
- Why physical data transfer devices exist
- The three Snow Family devices and their use cases
- Edge computing with Snow devices
- Key exam facts

---

## What is the AWS Snow Family?

The AWS Snow Family is a collection of **physical devices for transferring large amounts of data to and from AWS** when network transfer is too slow, too expensive, or impractical. AWS ships a physical appliance to your location; you copy data to it; you ship it back; AWS loads the data into S3.

---

## Why not just use the internet?

Consider moving 100 TB of data to AWS over a 1 Gbps internet connection:

- 100 TB = 100,000 GB
- At 1 Gbps (125 MB/s theoretical, ~50 MB/s realistic): 100,000 GB ÷ 0.05 GB/s = ~2,000,000 seconds ≈ **23 days**

In practice, data transfers this large take weeks or months over the internet and consume bandwidth that affects other operations. A Snow device can be loaded in a day and shipped overnight.

---

## The three Snow Family devices

**AWS Snowcone** — The smallest device. 2 vCPUs, 4 GB RAM, 8 TB usable storage. Can be carried in a backpack. Designed for edge locations — collecting data from sensors, surveillance cameras, or field locations. Can also be used for small data transfers back to AWS.

**AWS Snowball Edge** — A suitcase-sized device for larger transfers and edge computing. Two variants:
- **Snowball Edge Storage Optimized**: 80 TB usable storage — focused on large data transfers
- **Snowball Edge Compute Optimized**: 104 TB storage + significant compute (52 vCPUs, 208 GB RAM, optional GPU) — for edge computing where you process data locally before sending to AWS
- Use case: data migration, edge analytics, content distribution, IoT data collection

**AWS Snowmobile** — A shipping container on a semi-truck, with up to **100 PB** of storage. For extremely large migrations (entire data centers). AWS sends the truck to your facility, you fill it, and it's driven to an AWS data center.

---

## Edge computing

Snow devices can run **EC2 instances and Lambda functions locally** at the edge — processing data where it's collected, before sending it to AWS. Useful in:
- Remote locations (oil fields, ships, military) with limited connectivity
- Manufacturing plants that process sensor data locally
- Content distribution at remote sites

---

## Snow Family comparison

| Device | Storage | Compute | Use case |
|--------|---------|---------|---------|
| Snowcone | 8 TB | Minimal | Small transfers, field edge data collection |
| Snowball Edge Storage | 80 TB | Moderate | Large data migrations |
| Snowball Edge Compute | 104 TB + GPU | High | Edge computing + data migration |
| Snowmobile | 100 PB | None | Exabyte-scale data center migration |

---

## Exam focus

- Snow Family = **physical devices for offline data transfer** when network transfer is impractical
- **Snowcone** = small (8 TB), backpack-portable
- **Snowball Edge** = mid-size (80–104 TB), edge computing capable
- **Snowmobile** = truck-scale (100 PB), data center migrations
- Also support **edge computing** (run EC2/Lambda locally at the device)
- Use when exam describes: "transfer petabytes of data offline," "too much data for internet transfer," "no reliable network connection"

---

## Practice questions

**Q1.** A media company needs to migrate 500 TB of video archives from their on-premises data center to Amazon S3. Their internet connection is 200 Mbps, making an online transfer impractical. Which AWS service provides a physical device to move this data?

A) AWS Direct Connect  
B) AWS DataSync  
C) AWS Snowball Edge  
D) AWS Transfer Family

**Answer: C** — Snowball Edge (Storage Optimized) devices hold 80 TB each; multiple devices can be ordered in parallel for 500 TB. Data is loaded onto the devices on-site and shipped to AWS for loading into S3. Direct Connect is a dedicated network link — faster than internet but still an online transfer. DataSync automates online data transfer. Transfer Family supports SFTP/FTP to S3 but still relies on network transfer.
