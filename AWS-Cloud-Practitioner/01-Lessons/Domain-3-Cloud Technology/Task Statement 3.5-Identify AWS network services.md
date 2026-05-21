# Task Statement 3.5: Identify AWS Network Services

## What you'll learn
- VPC — your private network in AWS
- How traffic gets to your application from the internet
- DNS with Route 53
- Load balancing
- Connecting your on-premises network to AWS
- Improving global application performance

---

## Amazon VPC — Virtual Private Cloud

A VPC is your private, isolated section of the AWS network. Every resource you create runs inside a VPC. You define the IP address range, create subnets, control routing, and decide what connects to the internet.

VPC was already covered in depth in Domain 2 (security context). The key networking points:

- **Subnets** divide your VPC into segments. Public subnets route to the internet; private subnets don't.
- **Route tables** define where traffic goes. Each subnet is associated with a route table.
- **Internet Gateway** is the VPC's connection to the public internet. Required for any resource that needs to be internet-reachable.
- **NAT Gateway** allows resources in private subnets to initiate outbound internet connections (for downloading software updates, calling external APIs) without being directly reachable from the internet.

**VPC Peering**: Connect two VPCs so resources in each can communicate privately. Traffic never leaves the AWS network.

---

## Elastic Load Balancing (ELB)

A load balancer distributes incoming traffic across multiple targets — EC2 instances, containers, Lambda functions, IP addresses. Without a load balancer, a single server handles everything, creating a bottleneck and single point of failure.

AWS provides three types of load balancers:

**Application Load Balancer (ALB)**
Operates at Layer 7 (HTTP/HTTPS). Can route traffic based on URL path, hostname, query string, or HTTP headers. The standard choice for web applications and microservices.
- *Example*: Route requests to `/api/*` to one group of servers, and requests to `/images/*` to another.

**Network Load Balancer (NLB)**
Operates at Layer 4 (TCP/UDP). Extremely high performance — handles millions of requests per second with ultra-low latency. No HTTP routing rules; routes based on connection-level information.
- *Use when*: High-performance TCP applications, gaming, financial trading, real-time communications.

**Gateway Load Balancer (GWLB)**
Used to deploy and scale third-party network appliances (firewalls, intrusion detection systems) in a VPC. Transparent to traffic flow.

All ELB types automatically distribute traffic across multiple Availability Zones — key for high availability.

---

## Amazon Route 53

Route 53 is AWS's managed DNS service. DNS is the system that translates domain names (www.example.com) into IP addresses (192.0.2.1) that computers use to communicate.

**What Route 53 does**:
- **Domain registration**: Buy and manage domain names directly in AWS
- **DNS hosting**: Host DNS records for your domain
- **Health checks**: Monitor endpoint health and route traffic away from unhealthy endpoints
- **Traffic routing**: Route users to different endpoints based on policies

**Route 53 routing policies** (exam-tested):

| Policy | What it does |
|--------|-------------|
| Simple | Route to a single resource |
| Weighted | Split traffic by percentage (10% to new version, 90% to old) |
| Latency | Route users to the Region with lowest latency for them |
| Failover | Route to primary; if it fails, automatically route to secondary |
| Geolocation | Route based on user's geographic location |
| Geoproximity | Route based on distance to AWS resources |
| Multi-value | Return multiple IP addresses; basic load distribution |

The combination of health checks + failover routing creates automatic disaster recovery: if your primary endpoint goes down, Route 53 detects it and redirects traffic to a backup within seconds.

---

## Amazon CloudFront

CloudFront (covered in Task 3.2) is the CDN service. In the networking context: it sits between your users and your origin (S3, an ALB, a custom server), caches content at Edge Locations, and reduces origin load.

**HTTPS everywhere**: CloudFront provides SSL/TLS certificates (via AWS Certificate Manager) and enforces HTTPS between users and Edge Locations, and between Edge Locations and your origin.

**Origin types**: CloudFront can pull from S3 buckets, ALBs, EC2 instances, API Gateway, or any HTTP server.

---

## AWS API Gateway

API Gateway is a managed service for creating, publishing, securing, and monitoring APIs. It's the front door for applications to access backend services.

**What it does**:
- Accept API requests from clients (mobile apps, web browsers, other services)
- Route them to backends: Lambda functions, HTTP endpoints, AWS services
- Handle authentication and authorization
- Throttle requests to prevent overload
- Cache responses
- Monitor usage and performance

**Common pattern**: Client → API Gateway → Lambda function → DynamoDB. A fully serverless web application backend with no servers to manage.

---

## AWS Direct Connect

A dedicated private connection between your on-premises network and AWS. Physical fiber, provisioned by an AWS Direct Connect Partner. No internet — traffic flows privately between your data center and AWS.

Already covered in Task 3.1. The networking-specific point: Direct Connect provides consistent, predictable bandwidth and low latency. For organizations transferring terabytes of data regularly or running latency-sensitive hybrid applications, the dedicated connection performs far better than internet-based VPN.

---

## AWS VPN

**Site-to-Site VPN**: Encrypted IPsec tunnel over the internet between your on-premises router and a Virtual Private Gateway in your VPC. Quick to set up (minutes vs. weeks for Direct Connect). Performance depends on your internet connection quality.

**Client VPN**: OpenVPN-based service for individual devices to connect to your VPC over the internet. For remote workers who need access to resources in a private subnet.

---

## AWS Transit Gateway

As AWS environments grow, connecting everything with individual VPC peering connections becomes unmanageable. A mesh of 10 VPCs requires 45 individual peering connections.

Transit Gateway acts as a central hub that connects VPCs, VPNs, and Direct Connect connections. Instead of individual peer connections, each network connects once to Transit Gateway — and can then communicate with all other connected networks.

Think of it like a hub-and-spoke model: Transit Gateway is the hub, VPCs and on-premises networks are the spokes.

---

## AWS Global Accelerator

Global Accelerator improves the availability and performance of your applications for global users. It assigns static IP addresses to your application and routes user traffic through the AWS global network (instead of the public internet) to the nearest healthy endpoint.

**How it differs from CloudFront**:
- CloudFront caches content at Edge Locations (better for static content, CDN)
- Global Accelerator routes to your application endpoint through the AWS backbone (better for dynamic content, non-HTTP protocols, consistent global routing)

*Use when*: Gaming, IoT, VoIP, or applications with users in multiple regions who need low-latency access to dynamic content that can't be cached.

---

## Exam focus

| Service | Purpose |
|---------|---------|
| VPC | Private isolated network; subnets, routing, gateways |
| Application Load Balancer | HTTP/HTTPS routing with path/header rules |
| Network Load Balancer | High-performance TCP/UDP load balancing |
| Route 53 | DNS, domain registration, traffic routing policies |
| CloudFront | CDN, Edge Location caching, HTTPS |
| API Gateway | Managed API endpoint for Lambda and backends |
| Direct Connect | Dedicated private fiber to AWS |
| Site-to-Site VPN | Encrypted tunnel over internet to AWS |
| Transit Gateway | Central hub connecting multiple VPCs and networks |
| Global Accelerator | Global routing via AWS backbone, static IPs |

---

## Practice questions

**Q1.** A company has a web application running in two AWS Regions: us-east-1 and eu-west-1. They want users to be automatically routed to the Region with lower latency. Which Route 53 routing policy should they use?

A) Simple  
B) Weighted  
C) Latency  
D) Failover

**Answer: C** — Latency-based routing directs users to the Region with the lowest network latency for them. Route 53 measures latency from the user's location to each Region and routes accordingly. Simple has no routing logic. Weighted splits traffic by percentage. Failover routes to a backup only when the primary is unhealthy.

---

**Q2.** A microservices application has separate services for authentication, product search, and order processing. All three services should be accessible under the same domain, routed by URL path: /auth, /search, and /orders. Which load balancer type handles this?

A) Network Load Balancer  
B) Application Load Balancer  
C) Gateway Load Balancer  
D) AWS Transit Gateway

**Answer: B** — Application Load Balancers support path-based routing rules: requests to /auth go to the auth service, /search to the search service, etc. NLB routes at the connection layer and doesn't inspect HTTP paths. Gateway LB is for network appliances. Transit Gateway connects networks, not HTTP routes.

---

**Q3.** A company has 8 AWS VPCs across different regions and a data center they need to connect together. They currently maintain individual VPN connections between each pair but find it unmanageable. What simplifies this?

A) VPC Peering  
B) AWS Direct Connect  
C) AWS Transit Gateway  
D) Amazon Route 53

**Answer: C** — Transit Gateway is a hub-and-spoke connector. Each VPC and the on-premises data center connects once to Transit Gateway. All connected networks can then communicate through the hub. VPC Peering creates direct connections between two VPCs — scaling to 8 VPCs plus a data center still creates a mesh. Direct Connect is a physical connection technology. Route 53 is DNS.

---

**Next:** [Task 3.6 — Storage Services](./Task%20Statement%203.6-Identify%20AWS%20storage%20services.md)
