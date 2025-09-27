# **ALLOWING SOME AUTHORIZED CLIENTS TO INVALIDATE CACHE**

## Recommended
- **API Gateway + 0️⃣ = authorized front door** (respects cache rules)  
  - Use **Require Authorization** in cache settings.  
  - Clients send **Cache-Control: max-age=0** to bypass cache.

## Not Recommended
- **STS = backdoor access** (bypasses the API)  
  - Giving clients direct DynamoDB access does **not** invalidate API Gateway cache.  
  - Bypasses API-level control, logging, throttling, and security.
 

# **Lambda Invocation Types**

- **Synchronous (RequestResponse)** ⏳: Caller waits, gets result immediately.  
- **Asynchronous (Event)** 🚀: Caller doesn’t wait, Lambda retries on failure. ---ASYNC IS ALSO CALLED AS **EVENT ** 
- **Destinations**: Handle success/failure of async invocations externally (SNS, SQS, Lambda).

- # AWS HTTPS Flow Simplified

## 1️⃣ Think of the “Layers”

| Layer | Who talks to whom | HTTPS / Certificate Notes |
|-------|-----------------|--------------------------|
| **Viewer → CloudFront** | Users accessing your website | Controlled via **Viewer Protocol Policy** (HTTPS Only / Redirect HTTP → HTTPS). **No ALB cert needed**. |
| **CloudFront → ALB (origin)** | CloudFront fetching content from EC2 / ALB | Use HTTPS here if needed. ALB must have a **trusted certificate** (ACM or 3rd-party). **No self-signed certs**. |
| **ALB → EC2** | Load balancer sending request to EC2 | Optional HTTPS internally. Usually HTTP is enough for internal traffic. |

---

## 2️⃣ Simple Rules

1. **Want HTTPS for your users?** → Focus on **CloudFront Viewer Protocol Policy**.  
2. **Want HTTPS between CloudFront and ALB?** → ALB needs a **trusted certificate**.  
3. **Never use self-signed certs in production** (browsers won’t trust it).  
4. **ALB does not come with a default cert** — you must attach one manually.  
5. **S3 / storing certs** → irrelevant for ALB or CloudFront HTTPS directly.  

---

## 3️⃣ Memory Hook

**“Viewer HTTPS → CloudFront policy; CloudFront→ALB HTTPS → ACM or CA cert; no self-signed, no defaults.”**


# Amazon ElastiCache

**Definition:** Amazon ElastiCache is a **fully managed in-memory caching service** that improves application performance by allowing you to retrieve information from fast, managed, in-memory data stores instead of relying entirely on slower disk-based databases.

---

# ElastiCache: Memcached vs Redis

| Feature                  | Memcached ✅                     | Redis ❌                          |
|---------------------------|---------------------------------|----------------------------------|
| Threading / CPU usage     | Multithreaded, uses multiple cores | Mostly single-threaded           |
| Scaling                   | Easy to scale out/in             | Requires sharding/replication    |
| Use case                  | Simple key/value cache           | Advanced features, persistence   |
| Key gotcha                | Best for multithreaded large nodes | Not ideal for multithreaded nodes|

**Memory Hook:** Memcached = simple, multithreaded, scalable; Redis = rich features but single-threaded.



