# AWS X-Ray Cheat Sheet (DVA Exam)

## Overview
- **Purpose**: Distributed tracing system to analyze & debug production/distributed apps.
- **Helps with**: Latency analysis, service maps, error tracing, debugging performance bottlenecks.

## Key Concepts
- **Segment**  
  - A single traced request (e.g., one HTTP request to your app).  
- **Subsegment**  
  - A unit of work within a segment (e.g., DB query, HTTP call, AWS SDK call).  
- **Service Map**  
  - Visual map of services/app interactions built from segments.  

## Annotations vs Metadata
- **Annotations**
  - Key–value pairs.  
  - Indexed by X-Ray → **searchable via filter expressions**.  
  - Example: `userId=123`, `operation="INSERT"`.  

- **Metadata**
  - Key–value pairs.  
  - **Not searchable**, stored only for context/debugging.  
  - Example: raw SQL query string, payload body.  

👉 Exam Tip: *Use **Annotations** for searchable fields, **Metadata** for extra details.*

## How to Enable X-Ray
1. **Elastic Beanstalk**  
   - Add `.ebextensions/debugging.config`:  
     ```yaml
     option_settings:
       aws:elasticbeanstalk:xray:
         XRayEnabled: true
     ```
   - Starts the X-Ray daemon/agent.

2. **Lambda**  
   - Enable X-Ray in function config.  
   - Automatically traces requests.  

3. **EC2 / ECS / On-prem**  
   - Install & run X-Ray daemon.  
   - Instrument code with SDK.  

## SDK Instrumentation
- Required to capture:
  - **HTTP calls** (internal/external).  
  - **AWS SDK calls**.  
  - **SQL queries** (MySQL, PostgreSQL).  
- Supported languages: Java, Node.js, Python, Go, .NET, Ruby.  






# AWS CloudWatch Cheat Sheet (DVA Exam)

## Overview
- **Purpose**: Monitoring, observability, and operational insights for AWS resources & applications.
- **Can monitor**: Metrics, logs, events, and traces (via X-Ray).
- **Use cases**:
  - Track resource utilization (CPU, memory, disk, network)
  - Detect anomalies & trigger alarms
  - Debug application issues

---

## 1. CloudWatch Metrics
- **Definition**: Numerical data points that represent the state of a resource over time.
- **Types**:
  - **AWS service metrics**: Built-in (EC2 CPUUtilization, RDS FreeStorageSpace, API Gateway 4XXError/5XXError, Lambda Duration)
  - **Custom metrics**: Push your own app metrics via SDK or CLI
- **Units**: Count, Seconds, Bytes, Percent, etc.
- **Statistic options**:
  - `Average`, `Sum`, `Minimum`, `Maximum`, `SampleCount`

### Key Metric Examples
| Service          | Metric Name             | Description |
|-----------------|------------------------|------------|
| EC2              | CPUUtilization         | Percent CPU used |
| Lambda           | Duration               | Execution time in ms |
| API Gateway      | 4XXError / 5XXError    | Client/server errors |
| RDS              | FreeStorageSpace       | Remaining storage |

---

## 2. CloudWatch Logs
- **Purpose**: Store, monitor, and analyze log files from AWS resources & apps.
- **Features**:
  - Log groups → collections of log streams
  - Log streams → sequence of log events
  - Can search, filter, and trigger alarms on log patterns
- **Enabling Logs**:
  - Must enable for services like API Gateway, Lambda
- **Useful for**:
  - Debugging errors
  - Tracking request/response payloads
  - Anomaly detection

---

## 3. CloudWatch Alarms
- **Definition**: Triggered when a metric crosses a threshold.
- **Can trigger**:
  - SNS notifications
  - Auto Scaling actions
  - EC2 actions (stop, terminate, reboot)
- **Alarm states**:
  - `OK` → metric is normal
  - `ALARM` → metric breached threshold
  - `INSUFFICIENT_DATA` → not enough data

---

## 4. CloudWatch Events / EventBridge
- **Purpose**: Detect changes in AWS resources and trigger actions.
- **Examples**:
  - EC2 state changes → trigger Lambda
  - Scheduled cron-like events → run periodic tasks
- **Integration**: Lambda, Step Functions, SNS, SQS

---

## 5. CloudWatch Dashboards
- **Purpose**: Visualize metrics & logs in a single pane.
- **Widgets**:
  - Line, stacked area, number, text, gauge
- **Supports**:
  - Metrics from multiple AWS services
  - Custom metrics
  - Multiple regions

---

## 6. CloudWatch and X-Ray
- CloudWatch integrates with **X-Ray** for tracing.
- Allows correlation between logs, metrics, and request traces.
- Useful to identify bottlenecks in distributed applications.

---

## 7. Common Exam Scenarios
- **Monitor EC2 CPU usage** → CloudWatch Metrics + Alarm
- **Debug Lambda errors** → CloudWatch Logs
- **Detect API Gateway high 5XX errors** → CloudWatch Metric + Alarm
- **Track periodic events** → CloudWatch Event / EventBridge
- **Visualize multiple metrics in one place** → CloudWatch Dashboard
- **Trace request latency across services** → CloudWatch + X-Ray

# AWS CloudTrail Cheat Sheet (DVA Exam)

## Overview
- **Purpose**: Continuous monitoring and auditing of AWS account activity.
- **Tracks**: API calls and actions taken on AWS resources.
- **Records**: Who did what, when, from where.
- **Use cases**:
  - Security analysis & auditing
  - Compliance & governance
  - Troubleshooting operational issues

---

## Key Concepts

### Trails
- A **trail** is a configuration that enables CloudTrail to deliver log files to S3.
- Types:
  - **Single-region trail** → logs only that region
  - **Multi-region trail** → logs across all regions
- Can **send logs to CloudWatch Logs** for monitoring & alarms.

### Events
1. **Management events**
   - Operations on AWS resources (create, modify, delete)
   - Examples: `CreateBucket`, `RunInstances`, `PutItem`
2. **Data events**
   - Resource-level API operations (read/write)
   - Examples:
     - S3 object-level: `GetObject`, `PutObject`
     - Lambda function-level: `InvokeFunction`
   - Data events **not enabled by default** (enable per bucket/function)

### Event Format
- JSON structure includes:
  - `eventTime`
  - `eventName`
  - `userIdentity`
  - `sourceIPAddress`
  - `awsRegion`
  - `requestParameters`
  - `responseElements`

---

## CloudTrail Logging
- Default: **Management events are logged automatically** in the region.
- To persist logs: create a **trail** → send to **S3 bucket** (and optionally CloudWatch Logs).
- Enable **log file encryption** using **SSE-KMS** for security.

---

## Integration
- **CloudWatch Logs** → monitor & create alarms on specific API calls or suspicious activity.
- **EventBridge** → trigger actions on specific API activity.
- **AWS Config** → detect configuration changes and compliance violations.

---

## Security & Compliance
- Immutable logs → cannot be deleted if S3 bucket has **versioning** & **MFA delete** enabled.
- Tracks **all IAM users, roles, and AWS services** calling APIs.
- Essential for audits, compliance frameworks (PCI, HIPAA, SOC2).

---

## Common Exam Scenarios
| Scenario | CloudTrail Feature |
|----------|------------------|
| Track S3 bucket object access | Enable **data events** for S3 |
| Detect EC2 instance changes | Management events |
| Real-time alert for sensitive API calls | CloudTrail → CloudWatch Logs → Alarm |
| Store logs for compliance | CloudTrail → S3 bucket (with encryption & versioning) |

---

## Notes
- **CloudTrail ≠ CloudWatch Metrics**: CloudTrail logs API activity; CloudWatch monitors resource metrics & application logs.
- Can integrate CloudTrail + CloudWatch + EventBridge for **full monitoring & alerting pipeline**.

# AWS X-Ray APIs (DVA Exam Note)

- **GetTraceSummaries** → Gets **trace IDs only** (lightweight, summary view).  
- **BatchGetTraces** → Gets **full trace details** (complete trace data).  

💡 **Memory Trick:**  
*“Summaries first, Batch later.”*  
- Use **Summaries** to collect trace IDs.  
- Use **Batch** to fetch the full traces.

---
👉 Extra (avoid confusion):  
- **GetServiceGraph** → Returns the **service map** (nodes & edges of services), *not* trace IDs.
- 

## 🔐 Security Services

### 1. **AWS WAF (Web Application Firewall)**
- Protects **HTTP/HTTPS traffic** for **CloudFront, ALB, API Gateway, AppSync**.
- Blocks/filters based on rules (e.g., SQL injection, XSS, IP blocklist).
- Works at **Layer 7 (Application Layer)**.

### 2. **Network ACL (NACL)**
- Stateless firewall at the **subnet level** (VPC).  
- Rules applied in **number order**.  
- Evaluates both **inbound and outbound** traffic.  
- Use for **network-level control** (IP, port, protocol).  

### 3. **AWS Firewall Manager**
- **Centralized management** of firewall rules across multiple accounts/resources.  
- Works with **AWS WAF, AWS Shield, and Security Groups**.  
- Helps enforce **organization-wide security policies**.  

### 4. **Amazon GuardDuty**
- **Threat detection service** using ML and threat intel.  
- Monitors **CloudTrail, VPC Flow Logs, DNS logs**.  
- Detects anomalies (compromised IAM credentials, crypto mining, unusual API calls).  
- **Does not block traffic** — only alerts.

---

## 🧠 Memory Hooks
- **WAF → Web (Layer 7)**  
- **NACL → Network (subnet, stateless)**  
- **Firewall Manager → Policy manager (centralized control)**  
- **GuardDuty → Guard/Detect (threat detection, alerts only)** 



# API Gateway – Throttling vs Timeout

## Throttling
- API Gateway enforces throttling automatically if too many requests come in.  
- When throttled → returns **429 Too Many Requests** (not 504).  

## Timeout (504 Gateway Timeout)
- Happens when:  
  - Integration (e.g., Lambda) doesn’t respond within allowed time.  
  - **API Gateway max timeout = 29 seconds**.  
  - Or backend is too slow/unavailable.
  - - ⚡ **Note:** If your Lambda runs longer than 29s, API Gateway will **time out**, even though the Lambda keeps running in the background.
    - 

# API Gateway – HTTP Status Codes
- **429 Too Many Requests**
- **504 Gateway Timeout**  

# AWS CloudWatch Key Metrics – DVA Exam

| Service        | Metric Name                   | Description / Usage                                           |
|----------------|-------------------------------|---------------------------------------------------------------|
| **Lambda**     | Invocations                   | Number of times the function was invoked                     |
|                | Duration                      | Time taken per invocation (ms)                                |
|                | Errors                        | Number of failed invocations                                  |
|                | Throttles                     | Number of invocation requests that were throttled             |
|                | IteratorAge                   | Age of the last record in a stream for stream-based invocations |
|                | DLQErrors                     | Errors sent to dead-letter queue                               |
| **API Gateway**| 4XXError                      | Client-side errors                                            |
|                | 5XXError                      | Server-side errors                                            |
|                | Count                         | Number of API requests                                        |
|                | Latency                       | Time from request to response (ms)                            |
| **ALB / ELB**  | RequestCount                  | Number of requests received                                   |
|                | HTTPCode_ELB_4XX / 5XX        | 4XX and 5XX errors                                           |
|                | TargetResponseTime            | Time for targets to respond (ms)                              |
|                | HealthyHostCount / UnHealthyHostCount | Health status of targets                               |
| **ECS / EC2**  | CPUUtilization                | CPU usage (%)                                                 |
|                | MemoryUtilization             | Memory usage (%)                                              |
|                | DiskReadOps / DiskWriteOps    | Disk read/write operations                                     |
|                | NetworkIn / NetworkOut        | Network traffic                                               |
| **RDS**        | CPUUtilization                | CPU usage (%)                                                 |
|                | FreeableMemory                | Memory available                                              |
|                | DatabaseConnections           | Number of connections                                        |
|                | ReadIOPS / WriteIOPS          | Disk I/O operations                                           |
|                | DiskQueueDepth                | Number of pending I/O requests                                 |
| **S3**         | NumberOfObjects               | Number of objects in bucket                                    |
|                | BucketSizeBytes               | Total storage size                                            |
|                | AllRequests / 4XX / 5XXErrors | Request count / errors                                        |




## Kinesis – DVA Exam Key Points

| Feature         | Data Streams                  | Firehose                               |
|-----------------|-------------------------------|----------------------------------------|
| **Purpose**     | Real-time streaming, custom processing | Capture & deliver streaming data automatically |
| **Management**  | Must manage shards & consumers | Fully managed, no shards/consumers    |
| **Processing**  | Custom (Lambda, apps)         | Optional transformation                |
| **Delivery**    | Manual via consumers          | Automatic to S3, Redshift, OpenSearch, Splunk |




# Kinesis Shards – Split, Add & Merge

- **Split Shard:** Divide an **existing shard** into 2 to handle high write load on that shard.  
- **Add Shard:** Increase **total stream capacity** by adding a new shard (not splitting), often used when overall stream throughput is too low.  
- **Merge Shard:** Combine **two adjacent shards** into 1 to reduce cost when traffic is low.  
💡 Memory: Split = redistribute load; Add = increase capacity; Merge = reduce cost.



# ⚠️ GOTCHA: DynamoDB Streams

**⏰ All DynamoDB Streams stream records last ONLY 24 HOURS!**  
- You can retrieve **changes for the last 24 hours**, but anything older is **gone forever**.  
- **Cannot extend retention** — no exceptions!  
💡 Memory: *“If it’s older than 24h, it’s dust!”*


# CloudWatch – Standard vs Detailed Monitoring

| Feature                  | Standard Monitoring           | Detailed Monitoring         |
|--------------------------|------------------------------|----------------------------|
| **Frequency**            | 5-minute intervals           | 1-minute intervals         |
| **Cost**                 | Free (basic EC2 metrics)     | Additional charges         |
| **Use Case**             | General trend monitoring     | Real-time / fine-grained alerts and scaling |

# CloudWatch Logs Agent – Quick Memory Hook
*“No agent = basic EC2 metrics; agent = logs & custom metrics in CloudWatch.”*



