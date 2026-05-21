# Domain 1: Cloud Concepts — Practice Questions

> 20 questions | ~24% of the CLF-C02 exam

---

**Q1.** A company currently purchases and maintains physical servers in its own data center. Which benefit of moving to AWS directly addresses the problem of spending money on hardware that sits idle most of the time?

A) High availability  
B) Trade fixed expense for variable expense  
C) Benefit from massive economies of scale  
D) Increase speed and agility

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

When you own physical servers, you buy for peak capacity and those servers sit idle 70-80% of the time. AWS's pay-as-you-go model means you only pay for what you actually use, eliminating idle hardware costs. "Trade fixed for variable expense" is exactly this benefit.

- A is wrong — high availability is about uptime, not idle hardware cost.
- C is partially related (AWS passes volume savings to you) but doesn't specifically address idle hardware.
- D is about deployment speed, not hardware cost.
</details>

---

**Q2.** A startup wants to test a new mobile app idea. They want to launch quickly and only pay for what they use, with the ability to shut everything down if the idea fails. Which characteristic of cloud computing makes this possible?

A) Fault tolerance  
B) Elasticity  
C) Agility  
D) Durability

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Agility is the ability to quickly spin up and tear down resources, enabling rapid experimentation at low risk. The startup can launch in minutes and walk away if the idea fails — no sunk hardware costs.

- B (Elasticity) is about scaling up/down based on demand, not about the speed of launching new experiments.
- A and D are about reliability and data persistence, not speed of innovation.
</details>

---

**Q3.** Which cloud deployment model describes a company that hosts some workloads in AWS while keeping sensitive financial data on their own on-premises servers?

A) Public cloud  
B) Private cloud  
C) Hybrid cloud  
D) Community cloud

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Hybrid cloud combines on-premises (private) infrastructure with public cloud services. The company uses both — sensitive data stays on-prem, other workloads run on AWS.

- A is when everything runs in the public cloud.
- B is a fully private/on-premises setup.
- D (community cloud) is shared infrastructure for specific organizations — not an AWS concept tested here.
</details>

---

**Q4.** A company needs to ensure their web application keeps running even if an entire AWS data center fails. Which AWS infrastructure concept should they use?

A) AWS Edge Locations  
B) Multiple AWS Regions  
C) Multiple Availability Zones  
D) AWS Local Zones

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Availability Zones (AZs) are physically separate data centers within a Region. If one AZ has a power failure, others keep running. Deploying across 2+ AZs provides high availability within a Region.

- B (Multiple Regions) would work but is excessive for a single data center failure and significantly more complex/costly.
- A (Edge Locations) only cache content for CloudFront — you can't run application servers there.
- D (Local Zones) are for ultra-low latency, not high availability.
</details>

---

**Q5.** Which of the following BEST describes the "economies of scale" benefit of AWS?

A) AWS allows customers to pay only for the resources they use  
B) AWS purchases hardware in massive quantities, reducing per-unit cost, and passes savings to customers  
C) AWS allows applications to automatically scale based on traffic  
D) AWS eliminates the need for customers to manage physical infrastructure

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Economies of scale means buying in bulk = cheaper per unit. AWS buys millions of servers, gets better deals from manufacturers, and charges less per unit than you'd pay buying hardware yourself.

- A is pay-as-you-go / variable expense.
- C is elasticity/auto-scaling.
- D is the managed infrastructure benefit (stop spending on data centers).
</details>

---

**Q6.** A company is migrating to AWS. They want to move their existing on-premises application to EC2 without making any changes to the application code or architecture. Which migration strategy are they using?

A) Replatform  
B) Refactor  
C) Rehost  
D) Repurchase

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Rehost (also called "lift and shift") means moving the application to the cloud exactly as-is — same code, same architecture, just running on AWS hardware instead of on-prem servers. Zero changes to the application.

- A (Replatform) involves minor optimizations like moving from self-managed MySQL to RDS, but no core code changes.
- B (Refactor) means redesigning the architecture to use cloud-native features.
- D (Repurchase) means replacing the application with a SaaS product.
</details>

---

**Q7.** According to the AWS Well-Architected Framework, which pillar focuses on the ability to run and monitor systems to deliver business value and continuously improve processes?

A) Reliability  
B) Performance Efficiency  
C) Operational Excellence  
D) Sustainability

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Operational Excellence is about running workloads effectively, gaining insights into their operation, and continuously improving processes and procedures. Key practices include automating operations, responding to events, and defining standards.

- A (Reliability) is about recovering from failures and meeting demand.
- B (Performance Efficiency) is about using resources efficiently and maintaining that efficiency as demand changes.
- D (Sustainability) is about minimizing environmental impact.
</details>

---

**Q8.** A large retail company runs 100x the normal load during their annual sale event for 2 weeks per year. The rest of the year, they use a small fraction of that capacity. Which cloud characteristic best addresses their needs?

A) Durability  
B) High availability  
C) Elasticity  
D) Fault tolerance

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Elasticity is the ability to acquire resources when you need them and release them when you don't. The retailer can scale up massively for 2 weeks and scale back down, paying only for what they use — instead of buying hardware to support peak load that sits idle 50 weeks a year.

- A is about data persistence and not being lost.
- B is about uptime, not scale.
- D is about continuing to operate despite failures.
</details>

---

**Q9.** Which perspective in the AWS Cloud Adoption Framework (CAF) focuses on ensuring that cloud adoption aligns with the organization's enterprise architecture, technology standards, and innovation goals?

A) Governance perspective  
B) Platform perspective  
C) Operations perspective  
D) Business perspective

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

The Platform perspective covers architecture patterns, cloud infrastructure, and technology standards. It ensures the cloud platform is built in a way that supports the organization's technical goals.

- A (Governance) is about managing IT investment, risk, and compliance.
- C (Operations) is about day-to-day management of cloud services.
- D (Business) is about aligning cloud investment to business outcomes.
</details>

---

**Q10.** A company wants to replace their on-premises CRM system with Salesforce hosted in the cloud, as part of their AWS migration project. Which migration strategy does this represent?

A) Rehost  
B) Replatform  
C) Repurchase  
D) Refactor

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Repurchase means replacing an existing application with a commercial off-the-shelf SaaS solution. Moving from a custom CRM to Salesforce is a classic example — you're buying a new product rather than moving your existing code.

- A (Rehost) = move existing app to cloud unchanged.
- B (Replatform) = small cloud optimizations, same application.
- D (Refactor) = redesign the architecture from scratch.
</details>

---

**Q11.** Which statement about AWS Regions is CORRECT?

A) Each AWS Region contains exactly one Availability Zone  
B) Each AWS Region is completely isolated from other Regions by default  
C) AWS Regions and Availability Zones are the same thing  
D) Data automatically replicates between AWS Regions for durability

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

AWS Regions are geographically isolated from each other. Data and resources in one Region are not automatically replicated to another — this is by design so customers control where their data lives (for compliance/data sovereignty reasons).

- A is wrong — each Region has multiple AZs (minimum 2, usually 3+).
- C is wrong — Regions contain multiple AZs.
- D is wrong — cross-region replication must be explicitly configured by the customer.
</details>

---

**Q12.** Which of the following are characteristics of cloud computing? (Select TWO)

A) Requires long-term hardware procurement contracts  
B) Resources can be accessed from any location with internet connectivity  
C) Capacity must be planned and purchased months in advance  
D) Resources can be provisioned and released on demand  
E) Physical servers must be managed by the customer

<details>
<summary>Answer & Explanation</summary>

**Answers: B and D**

Cloud computing is characterized by on-demand access to resources from anywhere with internet access, and the ability to provision and release resources as needed.

- A, C, E all describe traditional on-premises IT, which is exactly what cloud computing eliminates.
</details>

---

**Q13.** What is the purpose of the AWS Well-Architected Tool?

A) It automatically fixes architectural issues in your AWS environment  
B) It deploys infrastructure using pre-built templates  
C) It helps customers review workloads against AWS best practices and improve over time  
D) It monitors application performance in real time

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

The Well-Architected Tool is a self-service review mechanism. You answer questions about your workload and it evaluates your architecture against the six pillars, identifies risks, and provides improvement recommendations.

- A is wrong — it identifies issues but doesn't auto-fix them.
- B describes CloudFormation.
- D describes CloudWatch.
</details>

---

**Q14.** A company's application experiences very high traffic for 2 hours each morning and almost no traffic at night. Which pricing model would result in the LOWEST overall cost?

A) On-Demand Instances running 24/7  
B) Reserved Instances for full capacity  
C) A combination of Reserved Instances for baseline + On-Demand for peak  
D) Spot Instances for the peak period

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

The optimal strategy is to reserve the minimum (baseline) capacity at the discounted Reserved price, then use On-Demand for the 2-hour peak spikes. This avoids paying for full Reserved capacity you don't use overnight.

- A wastes money running full capacity all night.
- B pays for peak capacity 24/7 even when unused.
- D (Spot) is risky for critical morning peak traffic since AWS can interrupt them with 2 minutes notice.
</details>

---

**Q15.** Which AWS infrastructure component is specifically designed to bring AWS services closer to end users to reduce latency for content delivery?

A) Availability Zones  
B) Local Zones  
C) Edge Locations  
D) Wavelength Zones

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Edge Locations are CloudFront CDN cache points distributed globally (400+). They store cached copies of frequently requested content (videos, images, static files) so users get content from a nearby location, reducing latency dramatically.

- A (AZs) are for compute and storage, not CDN caching.
- B (Local Zones) extend AWS Regions to metro areas for low-latency compute.
- D (Wavelength Zones) are for 5G edge workloads within telecom networks.
</details>

---

**Q16.** According to the Well-Architected Framework's Sustainability pillar, what is the primary goal?

A) Reduce AWS monthly costs  
B) Minimize the environmental impact of cloud workloads  
C) Ensure data is backed up sustainably  
D) Use renewable energy for on-premises servers

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

The Sustainability pillar (added in 2021) focuses on reducing the environmental impact of cloud infrastructure — minimizing the carbon footprint by using resources efficiently, choosing energy-efficient services, and scaling to eliminate idle capacity.

- A is Cost Optimization, not Sustainability.
- C and D are not what this pillar means in the context of the framework.
</details>

---

**Q17.** A company is evaluating cloud computing for the first time. Which statement MOST ACCURATELY describes infrastructure-as-a-Service (IaaS)?

A) The cloud provider manages the application and all underlying infrastructure  
B) The customer accesses ready-to-use software hosted in the cloud  
C) The cloud provider delivers virtualized computing resources, and the customer manages the OS and applications  
D) The customer uploads code and the cloud provider handles scaling automatically

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

With IaaS (like EC2), the cloud provider manages the physical hardware, virtualization, and networking. The customer controls the operating system, runtime, and applications running on top.

- A describes SaaS.
- B describes SaaS.
- D describes PaaS or FaaS (serverless).
</details>

---

**Q18.** Which migration strategy involves the MOST significant changes to an application and typically delivers the greatest long-term cloud benefit?

A) Rehost  
B) Replatform  
C) Retain  
D) Refactor/Re-architect

<details>
<summary>Answer & Explanation</summary>

**Answer: D**

Refactor/Re-architect means redesigning the application from the ground up to be cloud-native — microservices, serverless, event-driven architectures. It requires the most effort but delivers the greatest scalability, cost optimization, and agility benefits.

The 7 Rs on a spectrum: Retire/Retain (no change) → Rehost (copy paste) → Replatform (tweak) → Repurchase (replace) → **Refactor (rebuild)**.
</details>

---

**Q19.** A company asks: "Which AWS service gives us the ability to run workloads in our own data center using AWS infrastructure and services?" What is the answer?

A) AWS Direct Connect  
B) AWS Local Zones  
C) AWS Outposts  
D) AWS VPN

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

AWS Outposts delivers actual AWS hardware (servers and racks) to your on-premises data center. You manage workloads on your premises but using AWS APIs, services, and infrastructure — ideal for low-latency on-prem requirements or data residency rules.

- A (Direct Connect) is a dedicated private network connection to AWS, not on-prem compute.
- B (Local Zones) are AWS-managed infrastructure in major cities, not in your data center.
- D (VPN) is a secure network tunnel, not compute infrastructure.
</details>

---

**Q20.** Which of the following benefits of cloud computing refers to the ability to deploy applications in multiple geographic regions with just a few clicks?

A) Elasticity  
B) Go global in minutes  
C) Agility  
D) High availability

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

"Go global in minutes" is one of AWS's six benefits of cloud computing. It refers to deploying your application to AWS Regions around the world with minimal effort, giving users better performance by being geographically close to them.

- A (Elasticity) is about capacity scaling.
- C (Agility) is about speed of experimentation and development.
- D (High availability) is about uptime and fault tolerance.
</details>
