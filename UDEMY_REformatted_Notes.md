# ⚡ IAM & AWS CLI – Notes

---

## IAM Users & Groups Hands-On
- IAM is a **global service**. Users are valid across all regions.
- IAM Policies JSON syntax:
  - `Version`, `Id`
  - `Statement`: `Sid`, `Effect`, `Principal`, `Action`, `Resource`

### IAM MFA Overview
- MFA = password + security device
- Devices:
  - Amazon Authenticator, Google Authenticator, Microsoft Authenticator
  - Universal 2nd Factor (U2F) keys: e.g., YubiKey (USB device)
  - Hardware Key Fob: Gemalto or SecurePassID (GovCloud US)

### IAM MFA Hands-On
- Password policy customization: min characters, password strength, expiration, etc.

### AWS CloudShell
- Command-line shell in browser to execute AWS CLI commands
- Customizable settings (font size, multiple tabs)

### IAM Roles for AWS Services
- Roles are intended for AWS services, **not humans**
- Common roles:
  - EC2 instance roles
  - Lambda function roles
  - CloudFormation roles

### IAM Security Tools
- **IAM Role**: identity granting permissions via IAM policies
- **Assume Role Policy (Trust Policy)**: defines who can assume the role
- **IAM Policy**: defines permissions
- Entities that can assume roles:
  - AWS services, IAM users, cross-account users, federated users, manual via CLI/SDK
- Use Cases:
  - Lambda Execution Role (logs)
  - EC2 Role (S3/DynamoDB access)
  - Cross-Account Role
  - Federated Role
- Roles use **temporary credentials**; IAM and Trust Policies work together

#### IAM Security Tools Cheat Sheet
- IAM Credentials Report – all users and credentials (account level)
- IAM Access Advisor – shows user permissions & last accessed services (user level)

### IAM Best Practices
- Avoid root account except for setup
- One physical user → one AWS user
- Assign permissions to groups, not individual users
- Enforce strong password policies
- Use MFA
- Use Roles for AWS services
- Use Access Keys for programmatic access (CLI/SDK)
- Audit with IAM Credentials Report and Access Advisor
- Never share IAM users or Access Keys

### Shared Responsibility Model for IAM
- AWS manages security **of** the cloud
- Customers manage security **in** the cloud (users, roles, policies)

---

# ⚡ EC2 Fundamentals – Notes

## EC2 Overview
- EC2 = compute + storage + load balancing + auto scaling
- Components:
  - Virtual Machines (EC2)
  - Virtual Drives (EBS)
  - Load Balancing (ELB)
  - Auto Scaling (ASG)
- Public IP changes on stop/start
- Instance types:
  - Naming: `m5.xlarge` → `m` = class, `5` = generation, `xlarge` = size
  - General Purpose: T3, M5
  - Compute Optimized: C5, C6g
  - Memory Optimized: R5, X1e
  - Storage Optimized: I3, D2
  - Accelerated Computing: P3, Inf1

### Security Groups
- Network-level firewall for EC2
- Multiple SGs per instance, multiple instances per SG
- Default: inbound blocked, outbound allowed
- Common ports:
  - 22 = SSH, 3389 = RDP, 21 = FTP, 22 = SFTP
  - 80 = HTTP, 443 = HTTPS

### EC2 Instance Roles
- Never store IAM keys on EC2
- Use IAM Roles instead

### EC2 Purchasing Options
- **On-Demand**: short workloads, pay per second
- **Reserved**: 1-3 years, upfront or partial
- **Convertible Reserved**: flexible type
- **Savings Plans**: commit $ usage
- **Spot**: cheap, short workloads, not guaranteed
- **Dedicated Hosts/Instances**: isolated hardware
- **Capacity Reservations**: reserve AZ capacity

---

# ⚡ EC2 Instance Storage

## EBS Overview
- Elastic Block Store = network-attached storage
- One volume → one instance at a time (some multi-attach rare)
- Multi volumes per instance
- AZ-bound (cannot attach cross-AZ directly)
- Free tier: 30 GB/month
- `Delete on Termination`: default root = checked, other volumes = unchecked

### EBS Snapshots
- Backup of volume at a point in time
- Can copy across AZs/regions
- Features:
  - Archive (75% cheaper)
  - Recycle bin for protection
  - Fast Snapshot Restore (costly)

### AMI (Amazon Machine Image)
- Prepackaged software, OS, monitoring
- Launch EC2 from public, marketplace, or custom AMI
- Create AMI: start EC2 → customize → stop → build AMI

### Instance Store
- Physical drive attached to EC2
- High I/O, ephemeral
- Use: buffer, cache, temp storage

### EBS Volume Types
- GP1/GP2: general purpose SSD
- IO1/IO2 Block Express: provisioned IOPS SSD
- ST1: HDD, frequently accessed
- SC1: HDD, infrequently accessed
- Multi-Attach: attach io1/io2 to 16 EC2 instances in same AZ

### Amazon EFS
- Elastic File System, managed NFS
- Multi-AZ, scalable, expensive
- Linux-only, encrypted at rest
- Use: content management, web serving, shared data

### EBS vs EFS vs Instance Store
- EBS = single EC2, persistent
- EFS = multi-EC2, persistent, scalable
- Instance store = ephemeral, high I/O

---

# ⚡ ELB + ASG – Notes

## High Availability & Scalability
- Vertical scaling = increase instance size
- Horizontal scaling (elasticity) = increase instance count
- HA = at least 2 AZs
- Scaling in/out = reduce/increase instances

### Load Balancers
- **Classic (CLB)**: Layer 4 & 7, HTTP/HTTPS/TCP/SSL
- **Application (ALB)**: Layer 7, advanced routing, microservices, Lambda
- **Network (NLB)**: Layer 4, TCP/UDP, static IP, high performance
- **Gateway (GWLB)**: Layer 3, security appliance integration

### Sticky Sessions
- Ensures user routed to same backend
- Cookie types:
  - Application-based
  - Duration-based

### Cross-Zone Load Balancing
- ALB = automatic, free
- CLB = optional, free
- NLB = manual, extra cost

### SSL Certificates
- Encrypt data in transit (not at rest)

### Auto Scaling Groups
- Scale In/Out automatically
- Scaling policies: dynamic, scheduled, target tracking
- Components:
  - Launch configuration/template
  - Desired capacity
  - Min/Max capacity
- Connection Draining / Deregistration delay

---

# ⚡ RDS, Aurora, ElastiCache

## RDS
- **Read Replicas**: asynchronous, read-only, cross-AZ/region
- **Multi-AZ**: synchronous, standby not readable, automatic failover

## Amazon Aurora
- MySQL & PostgreSQL compatible, managed
- Auto-scaling storage (10GB → 128TB)
- 6 copies across 3 AZs
- Up to 15 read replicas
- Failover < 30 sec
- Serverless & Global DB options
- Reader/Writer endpoints for load balancing

## RDS Proxy
- Fully managed DB proxy
- Connection pooling, multi-AZ, IAM authentication
- Reduces DB resource usage, avoids overload

## ElastiCache
- Managed Redis & Memcached
- Cache strategies:
  - Cache-Aside (Lazy Loading)
  - Write-Through
  - Write-Behind
  - Cache-Around
- Redis = multi-AZ, durable, read replicas
- Memcached = no replication, sharding

## MemoryDB for Redis
- Durable Redis-compatible in-memory DB
- Multi-AZ, ultra-fast, scalable

---

# ⚡ Route 53 – Notes

## DNS Overview
- Translates human-friendly hostnames → IPs
- Domain registrar: Route 53, GoDaddy
- DNS records: A, AAAA, CNAME, MX
- Hosted Zones:
  - Public: internet
  - Private: VPC only

## Route 53 Records
- **A Record**: domain → IP
- **CNAME**: alias → another domain (non-root)
- **Alias**: hostname → AWS resource (root or non-root)
  - Supports: ELB, CloudFront, API Gateway, S3 Website, etc.

## Health Checks & Routing Policies
- Simple, Weighted, Latency-based, Failover, Geolocation, Geoproximity, Multi-Value Answer
- Health checks:
  - Public: HTTP/HTTPS/TCP
  - Private: via CloudWatch
  - Calculated: combines multiple checks

## Third-Party Domains
- Registrar manages ownership, Route 53 manages DNS records

---

# ⚡ Amazon S3 – Notes

## Overview
- Not global; bucket names must be globally unique
- Bucket naming conventions:
  - 3–63 characters, lowercase letters & numbers
  - No uppercase, underscores, IPs, `xn--` prefix, `-s3alias` suffix

## Object Keys
- Full path + filename, e.g., `folder1/folder2/file1.txt`

## Security
- IAM policies, bucket policies
- 

# ⚡ AWS Notes – CloudFront, ECS/ECR/Fargate, Elastic Beanstalk

---

## 15️⃣ CloudFront

### Overview
- Content Delivery Network (CDN)  
- Caches content at **edge locations** → reduces latency  
- Provides **DDoS attack prevention**  
- **Origins** = backends connected to CloudFront  

### CloudFront vs S3 Cross-Region Replication (CRR)
| Feature | CloudFront | S3 CRR |
|---------|------------|--------|
| Purpose | Speeds up content delivery | Replicates data across regions |
| How | Uses global edge locations to cache S3 content | Automatically copies objects to another region |
| Use Case | Static websites, images, videos | Backup, disaster recovery, compliance |
| Key Difference | Improves delivery speed, **does not backup data** | Provides replication and backup, **does not improve speed** |

### Caching & Policies
- Each edge location has its own cache  
- **Cache Key** = hostname + URL path by default  
- On request: checks cache → if missing, fetches from origin → caches it  
- **Goal:** maximize cache hit ratio  

**Cache Key Customization:**  
- Can include headers, cookies, query strings  
- Controlled via **Cache Policy** (none, whitelist, all, etc.)

**Invalidation:** removes cached items before TTL expires  

**Origin Request Policy:**  
- Forwards extra headers/cookies/query strings to origin **without affecting cache key**  
- Use when origin needs info not used in caching  

**Key Difference:**  
- Cache Policy = defines **cache key**  
- Origin Request Policy = defines **what goes to origin**

### Signed URL / Cookies
- Secure, temporary access to **private content** via CloudFront  
- **Signed URL** = access **one file**  
- **Signed Cookie** = access **multiple files**  
- Restrict access by **IP, path, time**  
- Requires **CloudFront key pair**  

**S3 Pre-signed URL:**  
- Temporary direct access to S3 object using IAM permissions  
- Bypasses CloudFront, **no caching**  
- Use for **short-term direct S3 access**

**When to Use:**  
- CloudFront Signed URL → content behind CloudFront  
- S3 Pre-signed URL → direct, short-term access  

---

## 16️⃣ ECS, ECR & Fargate – Docker in AWS

### Docker / AWS Services
- **Amazon ECR:** container image repository (like DockerHub)  
- **Amazon ECS:** AWS container platform  
- **Amazon EKS:** managed Kubernetes service  
- **AWS Fargate:** serverless container platform (works with ECS/EKS)  

### ECS Launch Types
| Type | Who manages infra | Pros | Use Case |
|------|-----------------|------|----------|
| EC2 | You | Full control, optimize costs (Spot instances), background services | Need control, customize instances |
| Fargate | AWS | No infra management, pay per CPU/memory | Simple deployments, small teams, serverless containers |

### ECS Auto Scaling
- Uses **Application Auto Scaling**  
- Scale on CPU, Memory, ALB Request Count per target  
- Scaling types: **Target Tracking** (preferred), Step Scaling, Scheduled  
- Fargate: easier, serverless  
- EC2: requires scaling EC2 instances via ASG or **ECS Capacity Provider**

**Example Flow:** CPU ↑ → CloudWatch alarm triggers → desired task count ↑ → new task launches → if EC2, Capacity Provider adds instance  

**Exam Tip:** prefer Fargate or Capacity Provider for EC2

### Rolling Updates
- Update gradually → no downtime  
- Updates batch → wait for healthy → next batch  
- Used in ECS, Kubernetes, deployment tools

### ECS Task Definitions
- Metadata (JSON/UI) for **running containers**  
- Key fields: image, CPU, memory, ports, env vars, IAM task role, logging  
- EC2: define container & host ports (host port 0 = dynamic, ALB maps it)  
- Fargate: only container port needed, each task gets private IP  
- Env vars: hardcoded or from **SSM/Secrets Manager**  
- Up to 10 containers per task  
- Volumes: EC2 = instance storage, Fargate = ephemeral (20–200 GB)  

### ECS Task Placement (EC2 Only)
- Decides **where to place tasks** in cluster  
- Steps: check CPU/memory/ports → apply constraints → apply strategy  

**Strategies:**  
- binpack → pack tasks tightly (cost-saving)  
- spread → distribute evenly (AZs/instances)  
- random → random placement  

**Constraints:**  
- distinctInstance → one task per EC2  
- memberOf → use cluster query (e.g., t2 instances)

### Amazon EKS
- Runs Kubernetes on AWS  
- Launch Types: EC2 (you manage nodes) / Fargate (serverless)  
- Pods = Kubernetes units (like ECS tasks)  
- Node Types: Managed, Self-managed, Fargate  
- Storage: EC2 → EBS, EFS, FSx; Fargate → EFS only  
- Supports ALB/NLB for public/private access  

### AWS Copilot
- CLI tool to build, release, operate container apps  
- Deploys to App Runner, ECS, Fargate  
- Hides infra complexity  
- Supports multi-environment deployment  
- Integrates with CodePipeline, logging, health checks  
- Auto-scales infra, focus on app code  

### ECS Solution Architectures
- Refer slides for architecture patterns

---

## 17️⃣ AWS Elastic Beanstalk

### Deployment Modes
| Mode | Description |
|------|------------|
| All at once | Fastest, downtime, good for dev |
| Rolling | Update batches, runs below capacity, no extra cost |
| Rolling with additional batch | Keep full capacity, small extra cost |
| Immutable | Deploy new instances in temporary ASG, zero downtime, higher cost |
| Blue/Green | New environment → test → swap URLs, zero downtime |
| Traffic Splitting | Canary testing via ALB, automated rollback, zero downtime |

### Lifecycle Policy
- Max 1000 app versions per account  
- Delete old versions by age/count  
- Option to keep/delete S3 source bundles  
- Managed by Beanstalk service role

### Extensions (.ebextensions)
- Directory: `.ebextensions/` at source root  
- YAML/JSON `.config` files (e.g., logging.config)  
- Modify environment, add AWS resources (RDS, DynamoDB, etc.)  
- Resources deleted if environment deleted  

### CloudFormation Integration
- Beanstalk uses **CloudFormation stacks** internally  
- Provisions ASG, ELB, Security Groups, scaling policies  
- Extend via `.ebextensions`  

### Cloning
- Clone environment → exact copy (config + resources)  
- RDS data **not cloned**  
- Customize after cloning

### Migrations
- **Load Balancer:** cannot change ELB type on existing env → create new env → swap traffic  
- **RDS:** snapshot, enable deletion protection → create new env without RDS → point app → shift traffic → terminate old env

- # ⚡ AWS CloudFormation Cheat Sheet

## 1️⃣ Stacks
- Collection of AWS resources managed as a single unit.
- Create, update, delete stacks via templates.

## 2️⃣ Templates
- JSON/YAML file defining resources, parameters, outputs, etc.
- Mandatory section: `Resources` (AWS components).
- Use `!Ref` to reference parameters/resources.
- Resource type format: `AWS::Service::ResourceType`.
- Check AWS documentation for resource properties.

## 3️⃣ Parameters
- User inputs to customize templates.
- Types: `String`, `Number`, `CommaDelimitedList`, AWS-specific, SSM Parameter.
- Validation: `AllowedValues`, `AllowedPattern`, `Min/Max`, `NoEcho`.
- Reference with `!Ref ParameterName`.

## 4️⃣ Outputs
- Optional section to export values (e.g., VPC ID).
- Exported outputs can be imported into other stacks with `Fn::ImportValue`.
- Enables stack-to-stack communication.

## 5️⃣ Conditions
- Control resource/output creation based on conditions (e.g., environment type).
- Functions: `Fn::Equals`, `Fn::And`, `Fn::Or`, `Fn::Not`.
- Apply with `Condition: ConditionName` on resources/outputs.

## 6️⃣ Deletion Policy
- Controls what happens on resource/stack deletion.
- Types:
  - `Delete` (default) – resource is deleted.
  - `Retain` – resource preserved.
  - `Snapshot` – snapshot created before deletion (DBs, EBS).
- Note: S3 bucket delete fails if not empty.

## 7️⃣ Stack Policies
- JSON policy controlling update actions on resources.
- Protect critical resources from accidental updates.
- Default denies updates unless explicitly allowed.

## 8️⃣ Termination Protection
- Prevents accidental stack deletion.
- Must disable before deleting the stack.

## 9️⃣ Service Roles
- IAM role dedicated to CloudFormation.
- Allows CFN to create/update/delete resources using role permissions.
- Users need `iam:PassRole` to delegate role.

## 1️⃣0️⃣ Custom Resources
- Extend CloudFormation with Lambda-backed custom logic.
- Useful for unsupported resources or custom provisioning (e.g., empty S3 before delete).
- Defined with:
  ```yaml
  Type: Custom::ResourceName
  ServiceToken: <Lambda ARN>

  # AWS CloudFormation & Integration Notes

---

## 1️⃣1️⃣ StackSets
- Deploy/update/delete stacks across multiple accounts and regions.  
- Managed from an **administrator account** (often with AWS Organizations).  
- Changes applied simultaneously to all stack instances.

---

## 1️⃣2️⃣ Intrinsic Functions
- `!Ref` → reference parameter value or resource physical ID.  
- `!GetAtt` → get resource attribute (e.g., AZ, PublicIp).  
- `!FindInMap` → get value from mapping.  
- `!ImportValue` → import exported value from another stack.  
- `!Base64` → encode string in Base64 (e.g., UserData).  

**Condition functions:** `!If`, `!Equals`, `!And`, `!Or`, `!Not`  
**Others:** `!Join`, `!Sub`, `!Select`, `!Split`, `!Cidr`

---

## 1️⃣3️⃣ Rollbacks
- Default: on failure, stack creation/update **rolls back**.  
- Can disable rollback to preserve resources for troubleshooting.  
- Update failures roll back to last known good state.  
- On rollback failure, manually fix and call `ContinueUpdateRollback`.

---

## 1️⃣4️⃣ Capabilities
- `CAPABILITY_IAM`: acknowledge templates creating IAM resources without names.  
- `CAPABILITY_NAMED_IAM`: for IAM resources with explicit names.  
- `CAPABILITY_AUTO_EXPAND`: for templates with macros/nested stacks.  
- Must acknowledge in console or CLI (`--capabilities`), else `InsufficientCapabilitiesException`.

---

# ⚡ AWS Integration & Messaging: SQS, SNS & Kinesis

### Amazon SQS - Standard Queues
- Fully managed message queue to decouple apps and enable async processing.  
- Unlimited throughput and message volume.  
- **Delivery:** at-least-once (may have duplicates), best-effort ordering.  
- **Max message size:** 256 KB.  
- **Retention:** 1 min – 14 days (default 4 days).  
- Low latency (<10 ms).  

**Producers & Consumers**  
- Producers send messages (`SendMessage`)  
- Consumers poll/process/delete (`DeleteMessage`)  

**Scaling:**  
- Use multiple consumers; scale with EC2 Auto Scaling  
- Trigger scaling on CloudWatch metric `ApproximateNumberOfMessages`  

**Security:**  
- In-flight: HTTPS  
- At-rest: KMS encryption  
- Access control via IAM and SQS policies  

**Queue Access Policies**  
- Resource-based JSON policies attached to SQS queues  
- Allow cross-account access or AWS service access  

**Message Visibility Timeout**  
- Message becomes invisible to others for default 30s after receipt  
- Use `ChangeMessageVisibility` to extend processing time  

**Dead Letter Queues (DLQ)**  
- Stores messages that fail processing after `MaximumReceives`  
- DLQ type must match source queue type  
- Set long retention (e.g., 14 days) and optionally redrive to source  

**Delay Queues**  
- Postpone message visibility for up to 15 min  
- Set at queue level (default) or per message (`DelaySeconds`)  

**Long Polling**  
- Consumer waits (1–20s) if queue is empty  
- Fewer API calls → lower cost, less CPU usage  
- Queue level: `ReceiveMessageWaitTimeSeconds`  
- Use case: chat apps → immediate message delivery  

**SQS Extended Client**  
- For large messages (>256 KB), store payload in S3; send pointer in SQS  

**Key API Calls:** `CreateQueue`, `DeleteQueue`, `PurgeQueue`, `SendMessage`, `ReceiveMessage`, `DeleteMessage`, `ChangeMessageVisibility`  
- Batch APIs available to reduce cost  

**SQS FIFO Queues**  
- Maintains message order per `MessageGroupId`  
- Name must end with `.fifo`  
- Throughput: 300 msg/s (no batch), 3000 msg/s (batch)  
- Deduplication interval: 5 min (content-based or explicit ID)  
- Use cases: transaction order, chat threads, per-customer order processing  

---

### Amazon SNS
- **Pub/Sub:** Producer → SNS Topic → multiple subscribers  
- **Subscribers:** Email, SMS, HTTP(S), Lambda, SQS, Kinesis Firehose  
- **Limits:** ~12M+ subscribers/topic, 100K topics/account  
- **Security:** TLS in-flight, KMS at-rest, IAM + SNS policies  
- **Mobile push:** GCM, APNS, ADM  

**Fan-Out Pattern**  
- Producer sends 1 message → SNS topic → multiple SQS queues receive it  
- Benefits: decoupled, reliable, scalable, supports retries/delayed processing  
- Subscribers can filter messages using JSON policies  

---

### Amazon Kinesis Data Streams & Firehose
**Firehose**  
- Fully managed, near real-time streaming delivery  
- Sources: Direct PUT, Kinesis Agent, CloudWatch Logs/Events, IoT Core, EventBridge  
- Destinations: S3, Redshift, OpenSearch, Splunk, Datadog, custom HTTP  
- Features: Lambda transform, convert/compress formats, automatic scaling, pay-per-use  

**Key Configurations:**  
- Buffer Size: 5 MB (default) → bigger = efficiency, smaller = faster  
- Buffer Interval: 300s default (min 60s, max 900s)  
- Compression: optional  
- Encryption: optional  
- IAM Role: auto-created  

**Firehose vs Kinesis Data Streams**  

| Feature       | Firehose                | Data Streams                 |
|---------------|------------------------|-----------------------------|
| Delivery      | Near real-time         | Real-time                   |
| Scaling       | Auto                   | Provisioned/on-demand       |
| Storage       | None                   | Up to 1 year                |
| Replay        | No                     | Yes                         |
| Coding        | No consumer code       | Write consumer/producer     |
| Destinations  | AWS services, HTTP     | Custom consumers            |

---

# ⚡ AWS Monitoring & Audit: CloudWatch, X-Ray, CloudTrail

### CloudWatch Metrics
- Default metrics: automatically provided by AWS  
- Custom metrics: `PutMetricData` API  
- Dimensions: key/value (e.g., InstanceId, Env)  
- Storage resolution: standard 60s, high-res 1s/5s/10s/30s  
- CloudWatch Agent: pushes system metrics (RAM, disk, etc.)  
- Namespace: logical container for metrics  

### CloudWatch Logs
- **Log Groups:** app/service level  
- **Log Streams:** instance/container/file level  
- **Retention:** 1 day–10 yrs or never  
- **Encryption:** default or KMS CMK  
- **Sources:** EC2, ECS, Lambda, Beanstalk, API GW, CloudTrail, Route53  
- **Agents:** Logs Agent (logs only), Unified Agent (logs + metrics)  
- **Querying:** Logs Insights, Live Tail  
- **Export:** S3, Subscription Filters → Kinesis/Firehose/Lambda/OpenSearch  

### Metric Filters & Alarms
- Extract metrics from logs → trigger alarms  
- Alarm states: OK, INSUFFICIENT_DATA, ALARM  
- Actions: EC2 (stop, reboot), Auto Scaling, SNS, Lambda  
- Composite alarms: combine multiple alarms with AND/OR  
- High-res metrics supported  

### CloudWatch Synthetics
- Simulate user behavior (APIs, apps)  
- Runs on schedule (1–5 min)  
- Scripts: Node.js or Python  
- Logs failures in CloudWatch, trigger SNS alarms  

### CloudTrail vs CloudWatch vs X-Ray

| Service     | Purpose                                 |
|------------|----------------------------------------|
| CloudTrail | Auditing → who did what (API calls)    |
| CloudWatch | Monitoring → system/app performance    |
| X-Ray      | Debugging & tracing → request flow     |

**Integration workflow:**  
1. CloudWatch detects problem  
2. SNS notifies  
3. EventBridge triggers actions  
4. X-Ray shows root cause  
5. CloudTrail shows who/what caused change  

---

# ⚡ AWS Serverless

### Compute
- **AWS Lambda** – Run code without managing servers  
- **AWS Fargate** – Run Docker containers serverlessly  
- **AWS Step Functions** – Orchestrate workflows without servers  

### Databases
- **Amazon DynamoDB** – Serverless NoSQL database  
- **Aurora Serverless** – Relational database, scales automatically  

### Storage
- **Amazon S3** – Object storage, fully managed  

### Messaging & Streaming
- **Amazon SNS** – Pub/Sub notifications, auto-scaled  
- **Amazon SQS** – Managed message queues, auto-scaled  
- **Amazon Kinesis Data Firehose** – Real-time streaming, auto-scaled, pay-per-use  

### Identity & Access
- **Amazon Cognito** – Serverless user authentication & authorization



