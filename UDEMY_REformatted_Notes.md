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


# AWS Serverless & Other AWS Services – DVA Exam Notes

---

## 21️⃣ AWS Serverless: Lambda

### Lambda & CloudWatch Events / EventBridge
- EventBridge + Lambda can be used to create cron-like jobs (serverless).

### Lambda Event Source Mapping
- Lets Lambda poll streams/queues (SQS, Kinesis, DynamoDB Streams) and invoke functions with batches of records.
- Push-based services (S3, API Gateway, EventBridge, SNS) trigger Lambda directly, no mapping needed.

**Exam Trick:**
- Streams + Queues → Needs event source mapping  
- Events/Notifications → Direct trigger, mapping not needed

### Lambda Destinations
- Route results of asynchronous Lambda executions.
- On Success / On Failure → SNS, SQS, EventBridge, or another Lambda.
- More flexible than DLQ (DLQ only handles failures with SQS/SNS).

### Lambda Permissions
- **Execution Role:** Lambda’s permissions to AWS services  
- **Resource-based Policy:** Who can invoke the Lambda function

### Lambda Monitoring & X-Ray
- Logs automatically sent to CloudWatch Logs
- Use X-Ray for tracing execution & performance

### Lambda@Edge & CloudFront Functions
- **Lambda@Edge:** Runs at CloudFront Edge locations, replicated automatically  
  Use cases: auth at edge, dynamic content routing, URL rewrites, header manipulation  
  Limited runtimes (Node.js, Python), higher latency than CloudFront Functions
- **CloudFront Functions:** Lightweight JS functions at Edge, millisecond execution  
  Best for high-volume, simple logic: URL rewrites, header manipulation, basic auth  
  Cannot access other AWS services

**Exam Tip:**  
- High volume, lightweight header/URL logic → CloudFront Functions  
- Personalization, AWS service access, heavier compute → Lambda@Edge  
- Backend/regional service → plain Lambda

### Lambda Function Performance
- By default, not in VPC → can reach internet & AWS services  
- In VPC + private subnets → loses internet access unless NAT Gateway added  
- CPU-bound functions → increase RAM  
- Timeout: Default 3 sec, max 900 sec (15 min)  
- Execution context: temporary runtime reused across warm starts → DB connections outside handler can be reused

### Lambda Layers & Custom Runtimes
- **Layers:** Share code/dependencies across functions (max 5 layers)  
- **Custom Runtimes:** Your own language/runtime via a layer with bootstrap file

### Lambda File Systems
- **Ephemeral Storage (`/tmp`):** 512 MB default, up to 10 GB, lasts only for function execution  
- **Persistent Storage:** S3 (object storage), EFS (shared, up to PB scale)  

### Lambda Concurrency
- **Reserved concurrency:** Guarantees max concurrent executions; extra invocations wait/fail

### Lambda & CloudFormation
- Package dependencies with function (zip) or use Layers  
- Python/Node.js: `pip install -t .` in project folder, zip & upload

### Lambda Container Images
- Supports container images up to 10 GB (ECR)  
- Use AWS base images or custom images implementing Runtime API  
- Test locally with Runtime Interface Emulator  
- Best practices: multi-stage builds, cache stable layers, use single ECR repo

### Lambda Versions & Aliases
- **Versions:** Immutable snapshots  
- **Aliases:** Pointers to versions + traffic splitting (e.g., PROD → v5, BETA → v6 10%)

### Lambda Limits
- Memory: 128 MB – 10 GB  
- Timeout: max 900 sec  
- Env vars: 4 KB max  
- `/tmp` storage: 10 GB  
- Concurrency: default 1,000 (request increase if needed)  
- Code package: 50 MB compressed / 250 MB uncompressed  
- Larger dependencies → use Layers or container images

### Lambda Best Practices
- Heavy work outside handler (DB connections, SDK)  
- Use environment variables, encrypt sensitive ones with KMS  
- Minimize deployment package → use Layers  
- Avoid recursion (Lambda calling itself)

### Lambda Function URL
- Built-in HTTPS endpoint → direct Lambda invoke, no API Gateway needed  
- Auth: NONE (public) or AWS_IAM  
- Optional CORS  
- Limits: Same as Lambda (memory, timeout, payload, concurrency)

### Lambda vs API Gateway
| Feature | Function URL | API Gateway |
|---------|-------------|------------|
| Setup | Quick, built-in | More configuration |
| Auth | NONE / AWS_IAM | IAM, Cognito, custom authorizers |
| Features | Basic HTTP invoke, CORS | Transform requests/responses, caching, throttling, stages |
| Use case | Single Lambda endpoint | Complex API routing & versioning |
| Cost | Lightweight | More features, higher cost |

### Lambda & CodeGuru Integration
- Monitor & optimize serverless app performance using CodeGuru Profiler

### Lambda Deployment Methods
- **All-at-once / Big Bang:** Fast, risky  
- **Canary:** Small subset first, then expand  
- **Linear/Rolling:** Gradual shifts  
- **Blue/Green:** Parallel new version, instant switch  
- **Immutable:** Deploy to new servers/containers, rollback safe  
- **A/B Testing:** Route traffic to different versions

---

## 22️⃣ AWS Serverless: DynamoDB

### Basics
- Fully managed NoSQL DB (key-value & document store)  
- Scales automatically (On-Demand) or via provisioned capacity  
- Highly available & durable (multi-AZ)  

**Key Concepts:**
- Table → collection of items  
- Item → row/document  
- Attributes → key-value pairs  
- Primary Key → Partition Key (+ optional Sort Key)  
- Secondary Indexes:
  - **GSI:** Any attribute as PK/SK, cross-partition  
  - **LSI:** Same partition key, different sort key  

### Capacity & Throughput
- **Provisioned:** WCU = 1 write/sec/1 KB, RCU = 1 read/sec/4 KB (strong), 2 reads/sec/4 KB (eventual)  
- **On-Demand:** Auto-scaling, pay per request  
- **Transactions:** ACID, 2x capacity

### DynamoDB Streams
- Captures item-level changes (Insert, Modify, Remove)  
- Trigger Lambda per batch of changes  
- Requires Lambda role with DynamoDB read access

### TTL (Time To Live)
- Auto-delete items after expiration timestamp  
- No WCU consumed  
- Deletes appear in Streams

### DynamoDB Writes
- Concurrent writes → last write wins  
- Conditional writes (optimistic locking)  
- Atomic counters → increment/decrement atomically  
- Batch writes → multiple items in one operation

### CLI Tips
- `--projection-expression` → retrieve specific attributes  
- `--filter-expression` → client-side filtering  
- Pagination: `--page-size`, `--max-items`, `--starting-token`

### Large Objects / S3
- Store large objects in S3, metadata in DynamoDB  
- Trigger Lambda on upload to update DynamoDB metadata

### Sharding
- Avoid hot partitions → add random prefix/suffix to partition key

### Security
- IAM full access control  
- VPC endpoints for private access  
- Encryption: At rest (KMS), in transit (SSL/TLS)  
- Backup: PITR, manual backup/restore  
- Global Tables: multi-region replication (requires Streams)  
- Fine-grained access via federated login + conditions

### Session State / Caching
| Storage | Description | Use Case |
|---------|------------|----------|
| DynamoDB | Serverless key-value | Auto-scaling sessions, durable |
| ElastiCache | In-memory (Redis/Memcached) | Low-latency session store |
| EFS | Shared file system | File-based session state |
| EBS / Instance Store | Local storage | Single-instance cache |
| S3 | Object storage | Not ideal for session state |

---

## 23️⃣ AWS Serverless: API Gateway

### Architecture
- Fully managed service for creating, publishing, monitoring APIs (REST, HTTP, WebSocket)  
- Acts as "front door" to backend services (Lambda, EC2, Step Functions, 3rd-party APIs)  

**API Types:**
- REST → Feature-rich, usage plans, request/response transformations  
- HTTP → Lightweight, lower latency/cost  
- WebSocket → Real-time, stateful, full-duplex

**Core Concepts:**
- Resources & Methods → resource tree mapped to backend  
- Stages → dev/test/prod  
- Endpoints → Edge-optimized, Regional, Private  

**Security Options:**
- IAM policies, Lambda authorizers, Cognito, AWS WAF  
- SigV4 signing for authenticated requests

**Performance & Traffic Management:**
- Throttling, caching, canary deployments  
- Monitoring via CloudWatch, CloudTrail, X-Ray

**CI/CD & IaC:**
- CloudFormation, AWS SAM, CDK  
- Manage multiple stages, custom domains, traffic shifting

**Exam Focus:**
- REST vs HTTP vs WebSocket  
- Secure access patterns  
- CORS configuration  
- Backend integration  
- Deployment automation

---

## 24️⃣ AWS CI/CD Services
- **CodeCommit:** Git repo in AWS  
- **CodeBuild:** Build/test automation  
- **CodeDeploy:** Deployment, including Blue/Green  
- **CodePipeline:** CI/CD pipeline automation  
- **CodeArtifact:** Dependency repository  
- **CodeGuru:** ML-based code insights  
- **CloudShell:** Quick browser CLI

---

## 25️⃣ AWS SAM – Serverless Application Model

### Overview
- Framework on top of CloudFormation for serverless apps  
- Focused on Lambda, API Gateway, DynamoDB, Step Functions, S3, EventBridge  
- `template.yaml` → defines serverless resources  
- Shorthand:
  - `AWS::Serverless::Function` → Lambda  
  - `AWS::Serverless::Api` → API Gateway  
  - `AWS::Serverless::Table` → DynamoDB

### CLI Commands
- `sam build` → build dependencies  
- `sam local invoke` → run Lambda locally  
- `sam local start-api` → run API Gateway + Lambda locally  
- `sam package` → package & upload to S3  
- `sam deploy --guided` → deploy via CloudFormation

### Deployment & CI/CD
- CodeDeploy traffic shifting: Canary, Linear, All-at-once  
- Can integrate with CodePipeline

### SAM Policy Templates
- Prebuilt IAM policy snippets in `template.yml`  
- Examples:
  - `S3ReadPolicy` → Lambda reads S3 bucket  
  - `SQSPollerPolicy` → Lambda polls SQS  
  - `DynamoDBCrudPolicy` → CRUD on DynamoDB  
  - `SNSPublishPolicy` → Publish messages to SNS  
  - `VPCAccessPolicy` → Access VPC resources

---

## 26️⃣ AWS CDK (Cloud Development Kit)
- Define infrastructure using programming languages (Python, JS, Java, TS, C#)  
- CDK converts code to CloudFormation templates  
- Benefits: loops, conditions, functions, easier maintenance  
- Downside: extra layer to learn, still depends on CloudFormation

---

## 27️⃣ Cognito

### Overview
- Identity & Access Management for apps (web & mobile)  
- Handles sign-up, sign-in, MFA, password reset

### Components
- **User Pools:** Authentication, issues JWT tokens, supports social logins & SAML  
- **Identity Pools:** Authorization, grants temporary AWS credentials via STS  
- Map roles to authenticated vs unauthenticated users

### Exam Tip
- IAM → internal AWS users  
- Cognito → external app users  
- User Pool = Auth  
- Identity Pool = AuthZ + AWS creds

---

## 29️⃣ Advanced Identity

### STS
- Security Token Service → create temporary credentials  
- Key APIs: `AssumeRole`  
- Federation: external IdPs (SAML, Cognito, AD)  
- Common in CI/CD, cross-account Lambda, federated logins

### AWS Directory Services
- Managed Microsoft AD in AWS  
- Flavors: Managed Microsoft AD, AD Connector, Simple AD  
- Integrations: EC2 Windows, Workspaces, QuickSight, RDS SQL Server  
- Exam Hook: Windows workloads needing domain join → Directory Service

---

## 30️⃣ AWS Security & Encryption

### Encryption Overview
- **In Transit:** TLS/SSL  
- **Server-Side Encryption (SSE):** AWS manages encryption  
- **Client-Side Encryption:** You manage encryption keys

**Exam Tip:**  
- “Don’t trust AWS with keys” → Client-Side  
- “Easiest/managed” → Server-Side  
- Network transfer security → In Transit

---

## 31️⃣ AWS Other Services

| Service | Description | Notes |
|---------|------------|------|
| Athena | Serverless SQL query on S3 | BI, analytics, CloudTrail/VPC logs |
| Macie | Data security, PII detection | ML-based, compliance (GDPR/HIPAA) |
| SES | Send emails at scale | SMTP/API, sandbox vs prod |
| SNS | Pub/Sub messaging | Subscribers: SQS, Lambda, HTTP/S, Email, SMS |
| OpenSearch | Managed Elasticsearch fork | Search, log analytics, CloudWatch/Kibana dashboards |
| MSK | Managed Kafka clusters | Real-time streaming, IAM & CloudWatch integration |
| ACM | SSL/TLS certs | Free, auto-renewal, CloudFront/ELB/API Gateway integration |
| AppConfig | Application config management | Feature flags, gradual rollouts, CloudWatch integration |
| CloudWatch AppInsights | Monitor apps | Detect anomalies, errors, bottlenecks |




