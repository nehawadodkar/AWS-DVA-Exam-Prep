# AWS Notes

## CloudFormation
- Get a CloudFormation template sample.

## EC2
- EC2 instance types cheat sheet.

---

## IAM

### Tools
- **IAM Access Analyzer**: Detects if resources (S3, IAM roles, KMS keys) are shared externally or public. Generates findings to identify and fix unintended access.
- **IAM Access Advisor**: Shows the last time a user or role accessed AWS services. Helps remove unused permissions and enforce least privilege.

### Policy Types
- Identity-based policies
- Resource-based policies
- ACLs
- Permissions boundaries
- Organizations SCP (Service Control Policy)
- Session policies

### MFA
- Root account **must enable MFA**.
- Recommended: Virtual MFA or hardware MFA.
- MFA for IAM users: Virtual or hardware.
- SMS MFA deprecated for root.

### IAM Usage
- IAM is used as a certificate manager **only when ACM is not supported in a region**.

---

## Networking

### Network ACL (NACL)
- Operates at **subnet level**.
- **Stateless**: inbound and outbound traffic evaluated separately.
- Rules can **allow or deny** traffic.
- Applies to **all instances** in the subnet.
- **Memory tip:** NACL = Subnet, No memory (stateless), Can deny.

### Security Group (SG)
- Operates at **instance level**.
- **Stateful**: return traffic allowed automatically.
- Rules **only allow** traffic; deny not possible.
- Default: denies all inbound, allows all outbound.
- **Memory tip:** SG = Security group, Stateful, remembers replies.

### Route Table
- Determines how **subnet traffic is routed**.
- Each subnet must be associated with **one route table**.
- Contains **destination CIDR → target rules** (IGW, NAT, peering, local).
- Controls whether a subnet is **public or private**.
- **Memory tip:** Route Table = “Traffic map,” directs packets where they should go.

---

## AWS X-Ray
- **Purpose:** Traces requests as they travel through applications (microservices, Lambda, API Gateway).
- **Use case:** Find latency, performance bottlenecks, errors in distributed apps.
- **Key points:**
  - Shows end-to-end request flow.
  - Helps debug complex architectures with multiple services.
  - Visualizes latency, errors, and traces in one map.
- **Memory tip:** Think of X-Ray as a detective following each request through your system to see where it slows down or fails.

---

## Route 53 Record Types

| Record | Purpose |
|--------|---------|
| A      | Maps domain/subdomain to IPv4 address |
| AAAA   | Maps domain/subdomain to IPv6 address |
| CNAME  | Maps domain/subdomain to another domain name (cannot be root domain) |
| Alias  | AWS-specific; maps domain to AWS resources (ELB, CloudFront, S3). Can be root domain |
| MX     | Directs email traffic to mail servers |
| TXT    | Stores text data for verification/SPF/DKIM |
| SRV    | Defines port & hostname for specific services (e.g., VoIP, XMPP) |
| NS     | Specifies authoritative name servers for a domain/subdomain |
| PTR    | Reverse DNS; maps IP to domain name |
| SPF    | Often via TXT; specifies allowed mail servers |

**Memory tip:**
- A / AAAA → IP addresses  
- CNAME / Alias → point to another name or AWS resource  
- MX / TXT / SPF → email & verification  
- NS / PTR / SRV → infrastructure / service direction  

---

## API Gateway
- All APIs created with API Gateway **expose HTTPS endpoints only**.

---

## Amazon ECS
- **Task placement strategy:** Algorithm for selecting instances for task placement or tasks for termination.
- Strategies:
  - **binpack:** Place tasks based on least available CPU/memory. Minimizes instances in use.
  - **random:** Place tasks randomly.
  - **spread:** Place tasks evenly based on attribute key-value pairs, instanceId, or host.

---

## Amazon S3 Transfer Acceleration (TA)
- **Purpose:** Speeds up uploads/downloads to S3 over long distances using CloudFront edge locations.
- **How it works:**
  - Enable TA on an S3 bucket.
  - Use endpoint: `bucketname.s3-accelerate.amazonaws.com`
  - Clients send data to nearest CloudFront edge; routed over AWS backbone to S3.
- **When to use:** Users far from S3 bucket region, low-latency global transfers.
- **When not to use:** Clients near bucket; extra cost.
- **Memory tip:** 🌍 Global on-ramp → 🚀 Edge → 🛣 AWS highway → 🪣 S3 bucket  
- **Exam angle:** Uses CloudFront edge locations, **not CloudFront caching**.

---

# DynamoDB - AWS DVA Exam Notes

## Basics
- Fully managed NoSQL database (key-value & document).  
- Supports **Provisioned** and **On-Demand** capacity modes.  
- Global tables for multi-region replication.  
- Supports **ACID transactions**.  

---

## Capacity Units

### 1. Read Capacity Unit (RCU)
- 1 RCU = 1 strongly consistent read/sec for **1 item ≤ 4 KB**.  
- Eventually consistent read = **0.5 RCU**.  
- **Formula:**
  - Strongly consistent: `RCU = ceil(ItemSizeKB / 4) * ReadPerSecond`
  - Eventually consistent: `RCU = ceil(ItemSizeKB / 4) * ReadPerSecond * 0.5`

**Example:**  
- Item = 6 KB, 10 strongly consistent reads/sec → `RCU = 20`

### 2. Write Capacity Unit (WCU)
- 1 WCU = 1 write/sec for **1 item ≤ 1 KB**.  
- **Formula:** `WCU = ceil(ItemSizeKB / 1) * WritesPerSecond`

**Example:**  
- Item = 3 KB, 5 writes/sec → `WCU = 15`

---

## Indexes

### Local Secondary Index (LSI)
- Created **at table creation**.  
- Same **partition key**, alternate **sort key**.  
- Reads consume RCU from base table.  

### Global Secondary Index (GSI)
- Can be created **any time**.  
- Can have **different partition & sort key**.  
- Has **its own RCU/WCU**, independent of base table.  

---

## DynamoDB Streams
- Captures **item-level changes** (insert, modify, remove).  
- Can trigger **Lambda** for real-time processing.  
- Stream options: `KEYS_ONLY`, `NEW_IMAGE`, `OLD_IMAGE`, `NEW_AND_OLD_IMAGES`.  
- **DVA focus:** know concept & use case only.  

---

## DynamoDB Accelerator (DAX)
- **In-memory cache** for DynamoDB.  
- Reduces read latency from **ms → μs**.  
- Helps offload RCU consumption for **read-heavy workloads**.  

---

## Transactions
- Supports **ACID transactions**.  
- Transactional read/write consumes **2x capacity**.  

---

## Exam Memory Tips
- **RCU → 4 KB per read**, **WCU → 1 KB per write**.  
- **Eventually consistent read → half RCU**.  
- **Transactional read/write → 2x capacity**.  
- **LSI → same partition, alternate sort key**.  
- **GSI → alternate partition + sort key, own capacity**.  
- **DAX → in-memory cache, reduces read latency**.  
- **Streams → change log, trigger Lambda, know concept only**.


- **AWS SAM (Serverless Application Model)**: A framework to define, build, and deploy serverless applications using simplified templates for Lambda, API Gateway, DynamoDB, and other AWS resources.

- # AWS Messaging & Streaming - DVA Exam Notes

## 1. Amazon SQS (Simple Queue Service)
- Fully managed message queue for **decoupling microservices, distributed systems, and serverless apps**.
- **Queue types:**
  - **Standard Queue:** at-least-once delivery, best-effort ordering, unlimited throughput.
  - **FIFO Queue:** exactly-once processing, preserves order, limited throughput (up to 300 TPS with batching).
- **Visibility timeout:** time during which a message is invisible after being received; prevents multiple consumers from processing it simultaneously.
- **Dead-letter queue (DLQ):** stores messages that can’t be processed after multiple attempts.
- **Long polling:** reduces empty responses and lowers cost by waiting for messages if none are immediately available.
- **Key DVA points:** know **Standard vs FIFO**, DLQ, visibility timeout, long polling.

---

## 2. Amazon SNS (Simple Notification Service)
- Fully managed **pub/sub messaging** service.
- Sends messages to **multiple subscribers**: SQS queues, Lambda, HTTP/S endpoints, email, SMS.
- **Fan-out pattern:** publish to SNS → multiple SQS queues/Lambda functions receive the message.
- Supports **message filtering**: subscribers can filter messages based on attributes.
- **Durability:** messages stored redundantly across multiple AZs.
- **Key DVA points:** know **SNS → SQS fan-out**, delivery targets, message filtering, pub/sub pattern.

---

## 3. Amazon Kinesis
### 3a. Kinesis Data Streams
- Real-time streaming data ingestion for **analytics, processing, and storage**.
- **Shards** determine capacity:
  - 1 shard = 1 MB/sec write, 2 MB/sec read, 1,000 records/sec write.
- Supports **ordering within a shard**.
- Consumer applications can **read multiple times**; retention configurable (default 24h, up to 7 days).
- **Key DVA points:** know shards, ordering, multiple consumers, real-time processing.

### 3b. Kinesis Data Firehose
- Fully managed **delivery service** to load streaming data into **S3, Redshift, Elasticsearch, or Splunk**.
- Automatic **batching, compression, and format conversion**.
- No need to write custom consumer code.
- **Key DVA points:** know **Firehose = delivery/ETL**, vs Data Streams = custom processing.

### 3c. Kinesis Data Analytics
- Analyze **streaming data in real-time** using SQL or Apache Flink.
- Connects to **Data Streams or Firehose** as input.
- **Key DVA points:** real-time analytics without building custom apps.

---

## 4. Integration Patterns
- **SNS → SQS:** decouple producers and consumers, fan-out pattern.
- **Lambda → SQS/SNS:** serverless processing of messages/events.
- **Kinesis → Lambda:** real-time processing of streaming data.
- **DVA focus:** know which service is used for **queueing vs pub/sub vs streaming**.

---

## 5. Exam Memory Tips
| Service | Pattern | Ordering | Delivery | Notes |
|---------|--------|---------|---------|-------|
| SQS Standard | Queue | Best-effort | At-least-once | High throughput |
| SQS FIFO | Queue | Exactly | Exactly-once | Limited throughput, preserves order |
| SNS | Pub/Sub | N/A | Push to subscribers | Fan-out to multiple SQS/Lambda |
| Kinesis Data Streams | Streaming | Per shard | At-least-once | Real-time analytics |
| Kinesis Firehose | Streaming → Storage | N/A | Automatic | ETL to S3/Redshift |
| Kinesis Analytics | Analytics | N/A | N/A | SQL/Flink processing of streams |


 – You can decrease the stream’s capacity by merging shards

 – You can increase the stream’s capacity by splitting shards
# API Gateway - AWS DVA Exam Notes

## Request & Response Flow

Client
│
▼
Method Request
│ (parameters, headers, body validation)
▼
API Gateway
│
Integration Request
│ (maps method request → backend format)
▼
Backend (Lambda, HTTP, Mock, etc.)
│
Integration Response
│ (maps backend response → method response)
▼
Method Response
│ (HTTP status codes, headers, body to client)
▼
Client


---

## Key Concepts

### Method Request / Method Response
- **Client-facing layer**.
- **Method Request:** expected parameters, headers, query strings, body.  
- **Method Response:** HTTP status codes, headers, response models sent to client.

### Integration Request / Integration Response
- **Backend-facing layer**.
- **Integration Request:** transforms client request into backend-friendly format.  
- **Integration Response:** transforms backend output into client-facing response format.

### Memory Tip
- **Method = client-facing**  
- **Integration = backend-facing**  
- Think of it as **two translation layers** between client and backend.

## DynamoDB Streams - DVA Exam Notes (Easy Version)

- **What it is:** DynamoDB Streams is like a **“change tracker”** for your table.  
  - It records **every insert, update, and delete** for 24 hours.  
  - Useful for **real-time triggers** (Lambda), audits, or replication.

---

### Stream View Types (Super Simple)
| Type | What you get | When to use / Remember |
|------|-------------|-----------------------|
| **KEYS_ONLY** | Only the **primary key** of the changed item | Lightweight, just to know “which item changed” |
| **NEW_IMAGE** | The item **after the change** | Use when you need the **latest data** |
| **OLD_IMAGE** | The item **before the change** | Use for **rollback or auditing** |
| **NEW_AND_OLD_IMAGES** | Both **before & after** | Use when you want to **compare old vs new** |

---

### Memory Tricks
- **Keys Only → Just the ID** (think: “I just want to know which item changed”)  
- **New Image → Latest State** (think: “Give me the new version”)  
- **Old Image → Previous State** (think: “Give me the old version”)  
- **New + Old → Compare** (think: “Show me before AND after”)  

💡 Easy Tip: Streams = **“What changed and when”**, pick the view type based on **what you want to see**.


**AWS Direct Connect
**
Provides a dedicated, private network connection from your on-premises data center to AWS. More reliable and lower latency than the public internet.

**S3 Transfer Acceleration**

Speeds up long-distance uploads/downloads to S3 using CloudFront edge locations + AWS backbone network. Useful for large files and global users.


**🔹 Stage Variables (API Gateway)**

Live in API Gateway, per stage (dev, test, prod).

Used to customize backend integrations without changing code.

Passed to Lambda via mapping templates.

Example: tableName = DevTable in dev stage, ProdTable in prod.

👉 Think: per-stage config at API Gateway level.

**🔹 Lambda Environment Variables**

Live in Lambda, per function.

Key-value pairs accessible inside Lambda code.

Same across all API Gateway stages (unless you deploy separate Lambdas).

Good for secrets, DB names, ARNs.

**👉 Think: per-function config inside Lambda.**

**⚡ Memory hook:**

Stage variables = API Gateway’s way to change behavior per stage.
Env variables = Lambda’s way to configure itself.



| Feature | LSI (Local Secondary Index) | GSI (Global Secondary Index) |
|---------|----------------------------|-----------------------------|
| **Partition Key** | Same as base table | Can be different |
| **Sort Key** | Can be different | Can be different |
| **Creation** | Only at table creation | Can add anytime |
| **RCU/WCU** | Shares base table | Own separate provisioned capacity |
| **Write Cost** | Included in base table writes | Extra cost for writes to index |
| **Limit** | 5 per table | 20 per table (soft limit) |
| **Use Case** | Alternate sorting/filtering within same partition | Query using different access patterns |

**Memory Hook:**  
- **LSI = Local → Same PK, shares capacity**  
- **GSI = Global → New PK allowed, separate capacity**

# 🔹 AWS Lambda Concurrency Notes

| Concept | What it means | Numbers & Limits (DVA Exam) | Restaurant Analogy |
|---------|---------------|-----------------------------|--------------------|
| **Account Concurrency Limit** | Total concurrent executions allowed per AWS account per region | Default = **1,000** (can be increased) | Total tables in the restaurant |
| **Reserved Concurrency** | Guaranteed concurrency for a specific function | Max = **900** if account limit is 1000 (AWS always leaves 100 unreserved) | VIP tables booked for specific guests |
| **Unreserved Concurrency** | Shared pool for all functions without reserved concurrency | Always at least **100** kept aside | Leftover tables for walk-in guests |
| **Burst Concurrency** | Short-term spike handling before steady scaling | Handles sudden bursts (500–3000 depending on region) | Extra folding chairs brought in for a rush |
| **Concurrency 0** | Reserved concurrency set to 0 disables a function | Function cannot run, all requests throttled | All tables for that guest blocked |
| **Throttled Invocations** | Requests denied because no concurrency is available | Reported in CloudWatch `Throttles` metric | Guests turned away at the door |
| **Push vs Pull Event Sources** | Push = one event = one invocation; Pull = batched events | Push: S3, API GW, SNS. Pull: SQS, Kinesis, DynamoDB Streams | Push = one guest per seat; Pull = several guests seated together |

# 📝 AWS API Gateway Cheat Sheet – DVA Exam

## 1️⃣ What is API Gateway?
- Fully managed service to create, deploy, and manage **APIs** (REST, HTTP, WebSocket).
- Acts as a **front door** for applications to access backend services like Lambda, EC2, or HTTP endpoints.
- Handles:
  - **Traffic management**
  - **Authorization & authentication**
  - **Monitoring & logging**
  - **Throttling & caching**

---

## 2️⃣ Core Components

| Component      | Quick Notes                                     |
|----------------|------------------------------------------------|
| **API**        | Collection of resources & methods             |
| **Resource**   | URL path segment (`/users`, `/orders`)        |
| **Method**     | HTTP verb: GET, POST, PUT, DELETE, PATCH      |
| **Integration**| Backend connection (Lambda, HTTP, Mock, AWS) |
| **Stage**      | Deployment env: dev / test / prod             |
| **Deployment** | Snapshot of API config for a stage            |

---

## 3️⃣ Integration Types

| Type             | Notes / Use Case                                   |
|-----------------|--------------------------------------------------|
| **Lambda**       | Serverless, Proxy / Non-Proxy                     |
| **HTTP/HTTPS**   | Any HTTP endpoint                                 |
| **Mock**         | Returns static response for testing              |
| **AWS Service**  | Call S3, SNS, DynamoDB                            |
| **VPC Link**     | Private resources inside VPC                      |

---

## 4️⃣ Lambda Integration: Proxy vs Non-Proxy

| Feature            | Proxy Integration           | Non-Proxy Integration       |
|-------------------|---------------------------|----------------------------|
| Request Mapping    | Auto forwards all         | Must map manually          |
| Response Mapping   | Lambda returns full HTTP  | API Gateway maps output    |
| Flexibility        | Less flexible             | More flexible              |
| Use Case           | Simple serverless API     | Complex transformations    |

---


---

## 6️⃣ Exam Quick Tips

- **Authorization**: IAM / Cognito / Lambda Authorizer  
- **Stages & Deployment**: Required to make API callable  
- **Throttling & Caching**: Protect backend from spikes  
- **Custom Domain**: Map API stage to friendly URL  

---

# 📝 AWS CI/CD Visual Cheat Sheet – DVA Exam

# 📝 AWS CI/CD Flow – DVA Exam


## CI/CD Flow

Developer commits code  
↓  
CodeCommit (Source Control / Git repo)  
↓  
CodePipeline (Orchestrates workflow)  
- Monitors each stage  
- Automates workflow  
- Supports manual approvals  
↓  
CodeBuild (Build & Test)  
- Produces artifacts  
↓  
CodeDeploy (Deploy apps)  
- Targets: EC2 / Lambda / ECS / On-prem  
↓  
Application Live





## Roles Summary

| Service        | Role in CI/CD                       | Key Notes                         |
|----------------|-----------------------------------|----------------------------------|
| CodeCommit     | Stores code                        | Git-based, secure                |
| CodeBuild      | Build & test code                  | Produces artifacts               |
| CodePipeline   | Orchestrates workflow              | Passes artifacts, monitors stages|
| CodeDeploy     | Deploys apps                       | Blue/Green or in-place           |
| CodeStar       | Project management + CI/CD setup   | Optional, unified dashboard      |

## Exam Tips

- Pipeline = central hub integrating **source → build → deploy**.  
- **Artifacts** move automatically between stages.  
- Manual approvals can pause pipeline at any stage.  
- **Monitoring**: CodePipeline shows stage status; CodeDeploy shows deployment success/failure.  
- CodeStar simplifies project setup but is optional for exam.  




