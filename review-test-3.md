### AWS SAM Resource Types – Quick Revision

- **AWS::Serverless::Function** → Defines a Lambda function + event sources (S3, DynamoDB Streams, Kinesis, etc.).  
- **AWS::Serverless::LayerVersion** → Defines a Lambda layer version for shared libraries/runtimes, keeps old versions safe.  
- **AWS::Serverless::Api** → Defines an API Gateway resource; use when you need full control of API configuration.  


### CDA – Troubleshooting and Optimization

**Question:**  
A reporting application is hosted in AWS Elastic Beanstalk and uses DynamoDB. The app currently **scans the entire table** to return data. The table is expected to grow due to new users and increased reporting requests.  
Which steps should be done to improve performance with **minimal cost**? (Select TWO.)

---

### ✅ Correct Answers
- **Use Query operations instead**  
  - Scans read the *entire* table.  
  - Queries are much faster and cheaper as they use keys/indexes to fetch only matching items.  

- **Reduce page size**  
  - Smaller `Limit` reduces the read capacity consumed per request.  
  - Helps avoid throttling and spreads load more evenly across requests.  

---

### ❌ Incorrect Options
- **Use DynamoDB Accelerator (DAX)**  
  - DAX caches **GetItem/Query** operations.  
  - Does **not** optimize Scans.  
  - Adds **extra cost**, not minimal.  

- **Set the ScanIndexForward parameter**  
  - Only changes the order of results.  
  - Does **not** improve performance.  

- **Increase the Write Capacity Units (WCU)**  
  - The workload is **read-heavy**, not write-heavy.  
  - Increases cost unnecessarily.  

---

### 🔑 Exam Tip
If the question mentions **Scan + minimal cost** → the best picks are:  
**Query operations + reduce page size.**  
Use **DAX** only after switching to Query/Key-based reads and if you need further acceleration.


### Improving DynamoDB App Performance – Minimal Code Change

**Scenario:** Single-item reads/writes causing network overhead.

**Easiest & Cost-Effective Solution:**
- Use **BatchGetItem** and **BatchWriteItem** to process multiple items per network call.
- Reduces round-trips, improves throughput, minimal code changes.

### S3 SSE-KMS Upload – Quick Flow


### CloudWatch EC2 Metrics – Memory Trick

- **Fact:** CloudWatch **does NOT monitor memory, swap, or disk space** by default.  
- **Solution:** Install the **CloudWatch Agent** on your EC2 instances to track these metrics.  

**Memory Trick:**  
- Think: **“CloudWatch sees CPU & network, but not what’s in the closet (memory, swap, disk)”** → install the agent to peek inside the closet.

### SQS Key Concepts – Quick Comparison

| Concept | One-Liner | Use Case / Notes |
|---------|------------|-----------------|
| **Delay Queue** | Delays delivery of new messages for a specified time. | Use when you want to postpone processing (e.g., order confirmation delay). |
| **Short Polling** | Immediately returns messages available (may return empty). | Simple, low-latency checks but may miss messages; cheaper for low traffic. |
| **Long Polling** | Waits up to 20s for messages if none are immediately available. | Reduces empty responses, saves cost, improves efficiency; good for steady workloads. |
| **Visibility Timeout** | Hides a message after a consumer picks it, preventing duplicate processing. | Ensure messages aren’t processed multiple times; adjust based on processing time. |

### Memory Trick:
- **Delay queue → “pause before showing”**  
- **Short poll → “peek quickly”**  
- **Long poll → “wait patiently for message”**  
- **Visibility timeout → “lock message while working”**



- **Small file (<5 MB)** → single PUT → S3 generates data key, encrypts file → **no kms:Decrypt needed**.  
- **Large file (≥5 MB)** → multipart upload → each part gets a data key → S3 encrypts each part → **kms:Decrypt + kms:GenerateDataKey* needed** to finalize multipart upload.  
- S3 handles all encryption internally; uploader just needs correct KMS & S3 permissions.
- 
### Amplify vs Elastic Beanstalk

- **Amplify** → For **front-end & serverless full-stack apps** (React, Vue, mobile + APIs).  
- **Elastic Beanstalk** → For **traditional backend apps** (Java, .NET, Node.js, Python) on EC2 with autoscaling.

- ### S3 Replication – CRR vs SRR

- **CRR (Cross-Region Replication)**  
  → Replicates objects to a **different AWS Region**.  
  → Use cases: **DR, compliance, latency for global users**.  

- **SRR (Same-Region Replication)**  
  → Replicates objects to another bucket in the **same Region**.  
  → Use cases: **staging vs prod separation, log aggregation, backup**.  

**Common Rules (both):**  
- Need **Versioning ON** (source & destination).  
- Replication is **asynchronous**.  
- Replicates **new objects only** (unless backfill configured).  

💡 **Memory Trick:**  
- **CRR = Copy Remote Region**  
- **SRR = Same Region Replica**


💡 **Memory Tip:**

- **AWS X-Ray (X-Ray)** = AWS-only tracing  
- **AWS Distro for OpenTelemetry (ADOT)** = Multi-backend, open-standard tracing

💡 **Super-short Memory Trick:**  
- **Standard = Fast, Can occur, at-least-once delivery, order not guaranteed**  
- **FIFO = Ordered, Exactly-once processing, deduplicated, limited throughput**



