# Task Statement 3.2: Define the AWS Global Infrastructure

## What you'll learn
- How Regions, Availability Zones, and Edge Locations are structured
- Why multi-AZ and multi-Region architectures matter
- How CloudFront, Local Zones, and Wavelength Zones extend AWS to users
- How to choose the right Region for your workload

---

## The three-layer structure

AWS's global infrastructure has three layers: Regions at the top, Availability Zones (AZs) inside each Region, and Edge Locations distributed everywhere.

Understanding how these relate to each other is essential for the exam — and for designing systems that actually stay up.

---

## AWS Regions

A Region is a geographic area that contains two or more Availability Zones. AWS currently operates in over 30 Regions worldwide (US East, US West, EU Frankfurt, EU Ireland, AP Singapore, AP Tokyo, and many more).

Each Region is fully independent. Resources in us-east-1 (Northern Virginia) have no automatic access to resources in eu-west-1 (Ireland). Data doesn't leave a Region unless you explicitly move it. This independence is important for data residency regulations — if a regulation requires EU customer data to stay in Europe, you deploy in an EU Region and it stays there.

### How to choose a Region

Four factors determine which Region to use:

**Compliance and data residency** — some regulations require data to remain in a specific country. This overrides all other considerations.

**Latency** — deploy near your users. A server in Virginia serving users in Tokyo will be noticeably slower than one in Seoul or Singapore.

**Service availability** — not all AWS services are available in all Regions. Newer services launch in us-east-1 first, then expand. If you need a specific service, check that it's available in your chosen Region.

**Pricing** — the same service costs different amounts in different Regions. Running an EC2 instance in us-east-1 is cheaper than in ap-southeast-1. Usually a secondary consideration after the three above.

---

## Availability Zones

An Availability Zone is one or more physical data centers within a Region. Each AZ is isolated from the others — separate power, networking, and physical location (typically several miles apart). A flood, fire, or power failure that takes out one AZ does not affect others in the same Region.

AZs within a Region are connected by high-bandwidth, low-latency fiber links. Traffic between AZs is fast enough that applications can span multiple AZs transparently.

### Why multi-AZ matters

A single AZ is a single point of failure. If that AZ has an outage, your service goes down.

A multi-AZ deployment runs resources in two or more AZs simultaneously. If one AZ fails, the others keep serving traffic. This is how you achieve high availability on AWS.

Many AWS managed services have built-in multi-AZ support:
- **RDS Multi-AZ**: your database automatically replicates to a standby in another AZ; failover happens automatically in minutes
- **ELB**: distributes traffic across instances in multiple AZs
- **S3, DynamoDB**: redundant across multiple AZs by design

For EC2-based applications, you put instances in multiple AZs behind a load balancer. If one AZ fails, the load balancer routes traffic to instances in the other AZs.

---

## Edge Locations

Edge Locations are AWS infrastructure points distributed in cities around the world — far more of them than Regions or AZs. There are over 400 edge locations globally. They are smaller than data centers but positioned close to end users.

Edge Locations are used primarily by **Amazon CloudFront** (CDN) and **Route 53** (DNS). Instead of every user request traveling to your Region (which might be on another continent), content cached at a nearby Edge Location is served locally — much faster.

---

## Amazon CloudFront

CloudFront is AWS's Content Delivery Network. It copies your content — images, videos, static web files, API responses — to Edge Locations around the world. When a user requests that content, it's served from the closest Edge Location instead of from your origin server.

**Why it matters**: A user in Cape Town requesting content from your server in us-east-1 (Virginia) experiences ~200ms of round-trip latency just from the physical distance. If that content is cached in a CloudFront Edge Location in Africa, the latency drops to single digits.

**What CloudFront caches**: Static assets (images, CSS, JavaScript), videos, and even API responses. You configure how long content is cached before CloudFront checks the origin for a fresh version.

**Security feature**: CloudFront integrates with AWS WAF and Shield for DDoS protection at the edge — attacks are absorbed before they reach your origin servers.

---

## Local Zones

Local Zones are extensions of AWS Regions placed in major metropolitan areas that don't have their own full Region. They bring compute, storage, and database services physically closer to large population centers.

Use case: applications that need single-digit millisecond latency for users in a specific city. Media rendering, gaming, real-time applications. A Local Zone in Los Angeles brings AWS services closer to LA users without needing to build a full Region there.

---

## Wavelength Zones

Wavelength Zones embed AWS compute and storage inside telecom providers' 5G networks. Applications running in Wavelength Zones deliver ultra-low latency (single-digit milliseconds) to 5G devices.

Use case: mobile apps requiring near-zero latency — autonomous vehicles, AR/VR, real-time gaming on mobile devices.

---

## AWS Outposts

Outposts are racks of AWS hardware installed in your own data center. They let you run AWS services (EC2, EBS, RDS, ECS, etc.) on-premises using the same APIs, tools, and console you use in the cloud.

Use case: workloads that must stay on-premises due to compliance or latency requirements, but that benefit from a consistent hybrid architecture using AWS tools.

---

## Why geographic redundancy matters

Multi-AZ protects against data center failures. Multi-Region protects against regional disasters or outages.

Most applications require multi-AZ at minimum. Multi-Region is used for the highest-availability requirements — where even a brief regional outage is unacceptable. It's more complex and expensive, but some applications (global financial systems, healthcare platforms) require it.

AWS Regions are fully independent and don't share failure domains — an outage in one Region has no effect on another.

---

## Exam focus

| Concept | What to know |
|---------|-------------|
| Region | Geographic area; fully independent; data doesn't leave without explicit action |
| Availability Zone | One or more data centers within a Region; isolated power and networking |
| Multi-AZ | Resources in 2+ AZs for high availability; protects against single AZ failure |
| Edge Location | 400+ global points for CDN/DNS; used by CloudFront and Route 53 |
| CloudFront | CDN that caches content at Edge Locations for low-latency global delivery |
| Local Zones | Compute closer to specific metros for low-latency applications |
| Wavelength Zones | AWS inside 5G networks for ultra-low-latency mobile applications |
| Outposts | AWS hardware in your data center for on-premises AWS services |
| Region selection | Consider compliance, latency, service availability, pricing — in that order |

---

## Practice questions

**Q1.** A company's web application runs on a single EC2 instance in one Availability Zone. The operations team is concerned about availability. What should they do?

A) Move the instance to a Region closer to users  
B) Deploy instances in multiple Availability Zones behind a load balancer  
C) Enable CloudFront for the application  
D) Use a dedicated host instead of a shared instance

**Answer: B** — Deploying across multiple AZs means a single AZ failure won't take down the application. A load balancer distributes traffic across the AZs. Options A and C improve performance, not availability. Option D changes the physical host model but doesn't help with AZ failure.

---

**Q2.** A media company serves high-resolution video to users in 40 countries. Videos are stored in S3 in us-east-1. Users in Europe and Asia complain about slow load times. What is the most effective solution?

A) Move the S3 bucket to a central Region like eu-central-1  
B) Create separate S3 buckets in every Region  
C) Use Amazon CloudFront to cache video content at Edge Locations near users  
D) Upgrade the EC2 instances serving the content

**Answer: C** — CloudFront caches content at 400+ Edge Locations worldwide. Users in Europe and Asia retrieve video from nearby edge nodes instead of from Virginia, eliminating the cross-ocean latency. Options A and B don't help users who are geographically far from any single Region. Option D doesn't address the underlying network distance problem.

---

**Q3.** A European company must ensure customer data never leaves EU territory due to GDPR requirements. What AWS feature enables this?

A) Enabling CloudFront for all data access  
B) Deploying in an EU Region — data stays within that Region by default  
C) Using an Availability Zone in a US Region with GDPR compliance  
D) Using AWS Outposts in a US data center

**Answer: B** — AWS Regions are independent. Data in an EU Region (Frankfurt, Ireland, Stockholm, etc.) does not replicate to other regions automatically. This data residency guarantee is a core AWS Region property. CloudFront is a CDN and distributes data globally by design. There are no US Regions with EU data residency.

---

**Next:** [Task 3.3 — Compute Services](./Task%20Statement%203.3-Identify%20AWS%20compute%20services.md)
