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


# CloudWatch Alarm Key Concepts

## 1️⃣ Evaluation Period
- **Definition:** Number of consecutive periods CloudWatch uses to evaluate a metric.  
- **Period:** Granularity of each metric data point (e.g., 1 min).  
- **Use Case:** Track HTTP 5xx errors every minute → set period = 1 min, evaluation periods = 3.  

## 2️⃣ Datapoints to Alarm
- **Definition:** Number of data points in the evaluation periods that must **breach the threshold** to trigger the alarm.  
- **Use Case:** Avoid false alarms for intermittent HTTP errors → set datapoints to alarm = 3/3 (all 3 consecutive minutes must exceed threshold).  

## 3️⃣ Example Scenario
- **Metric:** HTTP 5xx errors (custom metric)  
- **Period:** 1 minute  
- **Evaluation Periods:** 3  
- **Datapoints to Alarm:** 3  
- **Behavior:** Alarm triggers **only if errors exceed threshold for all 3 consecutive minutes** → prevents false alarms during intermittent spikes.

**Memory Hook:**  
**“Evaluation periods = how many periods checked; datapoints to alarm = how many must breach → ensures only consistent issues trigger the alarm.”**


# CloudFormation Lambda Deployment Flow --- Lambda Deployment (Quick Rev)

Local code → Package → S3 → Deploy → Stack → Lambda



# AWS Lambda X-Ray Environment Variables

## 1️⃣ Communication with X-Ray (Mandatory)
- `_X_AMZN_TRACE_ID`  
  - Auto-injected by Lambda per invocation  
  - Contains trace ID & parent segment info for SDK

- `AWS_XRAY_CONTEXT_MISSING`  
  - Defines behavior if trace context is missing  
  - Values:  
    - `LOG_ERROR` → log warning (debug-friendly)  
    - `RUNTIME_ERROR` → throw error (strict mode)

## 2️⃣ Optional / Informational
- `AWS_XRAY_TRACING_NAME` → custom name for trace segments  
- `AWS_XRAY_DEBUG_MODE` → not official, controlled via `AWS_XRAY_CONTEXT_MISSING`  
- `AUTO_INSTRUMENT` → not an AWS Lambda variable

## ✅ Memory Hook
"_X_AMZN_TRACE_ID = trace info, AWS_XRAY_CONTEXT_MISSING = how to handle missing trace context._"

# AWS Elastic Beanstalk Environment Types

| Environment Type          | Purpose                         | Key Characteristics / Use Case |
|---------------------------|---------------------------------|--------------------------------|
| **Web Server Environment** | Handles HTTP/S requests from end-users | - Receives traffic via Load Balancer (optional)  <br> - Runs web apps (Node.js, Python, Java, etc.) <br> - Auto-scales with demand <br> - Returns responses to clients |
| **Worker Environment**    | Handles background/asynchronous tasks | - Polls an **SQS queue** for tasks <br> - Processes tasks independently of web requests <br> - Ideal for email processing, media conversion, long-running jobs <br> - Auto-scalable separately |

## Memory Hook
*“Web Env = serves users; Worker Env = works in background from SQS.”*


# S3 Upload Optimization

- **Multipart Upload:** large files, split & parallel upload  
- **Transfer Acceleration:** long-distance uploads, uses CloudFront edges








