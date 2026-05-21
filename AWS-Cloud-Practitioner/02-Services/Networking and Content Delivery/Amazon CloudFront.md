# Amazon CloudFront

## What you'll learn
- What a CDN is and why it matters
- How CloudFront works with edge locations
- CloudFront use cases and integration with other AWS services
- Key exam facts

---

## What is Amazon CloudFront?

Amazon CloudFront is a **globally distributed Content Delivery Network (CDN)**. It caches copies of your content at edge locations around the world, delivering it to users from the nearest edge location rather than from your origin server — reducing latency and offloading traffic from your origin.

---

## How CloudFront works

**Origin** — The source of your content: an S3 bucket, EC2 instance, Application Load Balancer, or any HTTP server.

**Edge locations** — CloudFront has 450+ edge locations globally. When a user requests content, CloudFront routes the request to the nearest edge location.

**Cache hit** — If the requested content is in the edge location's cache (and not expired), CloudFront serves it directly from the edge — no request to the origin. Fast.

**Cache miss** — If the content isn't cached, CloudFront forwards the request to the origin, caches the response at the edge, and returns it to the user. Subsequent requests for the same content are served from the cache.

---

## What CloudFront accelerates

- **Static content**: Images, CSS, JavaScript, HTML — cache for hours or days at edge locations
- **Dynamic content**: API responses, personalized content — can still be routed through CloudFront for network optimization even if not cached
- **Video streaming**: Live and on-demand video delivery using HLS, DASH, and CMAF
- **Software downloads**: Large file distribution at scale

---

## Integration with other AWS services

- **S3**: CloudFront + S3 is the standard pattern for static website hosting — S3 stores the files; CloudFront delivers them globally
- **EC2 / ELB**: Use CloudFront in front of your web servers to offload static content and improve performance
- **Lambda@Edge / CloudFront Functions**: Run code at edge locations — customize responses, rewrite URLs, add authentication, without going back to origin
- **AWS Shield**: CloudFront is integrated with Shield Standard for DDoS protection — edge locations absorb attack traffic

---

## CloudFront vs Global Accelerator

| | Amazon CloudFront | AWS Global Accelerator |
|-|------------------|----------------------|
| Purpose | Cache and deliver content globally | Route traffic to optimal endpoints |
| Caching | Yes | No |
| Content type | HTTP/HTTPS (web content, video) | Any TCP/UDP application traffic |
| Benefit | Reduced latency for cached content | Consistent performance, static IPs |

---

## Exam focus

- CloudFront = **global CDN** — caches content at 450+ edge locations
- Reduces latency by serving from the edge nearest to the user
- Standard pattern: **S3 + CloudFront** for static website/content delivery
- Integrated with **Shield** for DDoS protection at the edge
- Use when exam describes: "deliver content to global users with low latency," "cache static files," "CDN," "S3 content delivery"

---

## Practice questions

**Q1.** A company hosts a static website on Amazon S3 and wants to serve it to users globally with low latency and reduced load on the S3 bucket. Which AWS service should they place in front of S3?

A) Amazon Route 53  
B) AWS Global Accelerator  
C) Amazon CloudFront  
D) AWS Direct Connect

**Answer: C** — CloudFront is a CDN that caches S3 content at edge locations globally, serving requests from the edge nearest to each user. This reduces latency and reduces the number of requests hitting the S3 origin. Route 53 handles DNS. Global Accelerator improves TCP/UDP routing but doesn't cache content. Direct Connect is for on-premises to AWS network connectivity.
