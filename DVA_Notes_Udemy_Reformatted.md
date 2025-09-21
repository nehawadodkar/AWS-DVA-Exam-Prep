# AWS Certified Developer Associate (DVA-C02) Udemy Notes

---

## 4. IAM & AWS CLI

### IAM Overview
- **IAM is a global service**—users and policies are valid across all regions.
- **Best Practices:**
  - Do not use the root account except for initial setup.
  - One physical user = one AWS user.
  - Assign users to groups; assign permissions to groups.
  - Enforce strong password policies and use MFA.
  - Use roles for AWS services.
  - Use Access Keys for programmatic access (CLI/SDK).
  - Audit permissions with IAM Credentials Report and Access Advisor.
  - Never share IAM users or access keys.

### IAM Users & Groups
- Users and groups are managed globally.
- Password policies can be customized (minimum characters, strength, expiration).

### IAM Policies
- **Policy JSON Syntax:** `Version`, `Id`, `Statement` (with `Sid`, `Effect`, `Principal`, `Action`, `Resource`).

### IAM MFA (Multi-Factor Authentication)
- MFA = password + a security device.
- **Supported Devices:** Amazon Authy, Google Authenticator, Microsoft Authenticator.
- **Universal 2nd Factor (U2F) Key:** YubiKey (physical USB device).
- **Hardware Key Fob:** Provided by Gemalto and SecurePassID for AWS GovCloud.

### IAM Security Tools
- **IAM Credentials Report:** Account-level report of all users and their credentials.
- **IAM Access Advisor:** Shows permissions granted and last accessed services for a user—useful for least privilege principle.

### IAM Roles
- **Purpose:** Used by AWS services (not physical users).
- **Common Roles:** EC2 instance roles, Lambda function roles, CloudFormation roles.
- **Role Components:**
  - **Assume Role Policy (Trust Policy):** Who can assume the role.
  - **IAM Policy:** Permissions for the role.
- **Entities That Can Assume a Role:**
  - AWS services, IAM users, cross-account users, federated users, manual assumption via CLI/SDK.
- **Use Cases:** Lambda execution, EC2 access, cross-account sharing, federated access.
- **Note:** Roles use temporary credentials and require trust policies.

### Shared Responsibility Model for IAM
- AWS and the customer share security responsibilities.

### AWS CloudShell
- Browser-based shell to run AWS CLI commands.
- Customizable settings (font size, tabs).

---

## 5. EC2 Fundamentals

### EC2 Basics
- EC2 is a suite of services:
  - Virtual machines (EC2)
  - Virtual drives (EBS)
  - Load balancing (ELB)
  - Auto scaling (ASG)

### EC2 Instance Types
- [AWS Instance Types](https://aws.amazon.com/ec2/instance-types/)
- **Naming Convention Example:** `m5.xlarge`
  - `m`: Instance class
  - `5`: Generation
  - `xlarge`: Size

#### Categories:
- **General Purpose (T3, M5):** Balanced use
- **Compute Optimized (C5, C6g):** Compute-heavy tasks
- **Memory Optimized (R5, X1e):** Memory-heavy tasks
- **Storage Optimized (I3, D2):** High I/O workloads
- **Accelerated Computing (P3, Inf1):** GPU/custom chip for ML/AI

### Security Groups
- Can be attached to multiple instances, and instances can have multiple security groups.
- Must create new security groups in a new region/VPC.
- Security groups act as network-level firewalls, filtering traffic before it reaches EC2.
- **Best Practice:** Separate security group for SSH access.
- **Troubleshooting:**
  - Timeout = Security group issue.
  - Connection refused = Application issue.
- **Defaults:** All inbound traffic blocked, all outbound allowed.
- **Classic Ports:**
  - 22: SSH
  - 3389: RDP
  - 21: FTP
  - 22: SFTP
  - 80: HTTP
  - 443: HTTPS

### EC2 Instance Connect
- Requires port 22 (SSH) inbound rule.

### EC2 Instance Roles
- Never enter IAM access keys on EC2 (`aws configure`). Use IAM roles instead.

### EC2 Purchasing Options
- **On-Demand:** Short, predictable workloads; pay per second.
- **Reserved:** 1/3 years, upfront/partial/no upfront; long workloads.
- **Convertible Reserved:** Long workloads; flexible instance type.
- **Savings Plans:** Commit to usage in $$, not instance type.
- **Spot:** Short, cheap, unreliable; batch/image processing.
- **Dedicated Hosts:** Full physical server, compliance, expensive.
- **Dedicated Instances:** No hardware sharing, no placement control.
- **Capacity Reservations:** Reserve AZ capacity; charged on-demand rates; manual cancellation required.

---

## 6. EC2 Instance Storage

### EBS (Elastic Block Storage)
- Acts like a "network USB stick".
- Attach to EC2 instances; one volume per instance (multi-attach rare).
- Must snapshot EBS to move across AZs.
- Free tier: 30GB/month.
- Specify size/IOPS upfront; can extend later.
- **Delete on Termination:** Default for root volume; configurable.

### EBS Snapshots
- Point-in-time volume backups.
- Can copy snapshots across AZs.
- **Features:**
  - Archive (75% cheaper, 24–72 hr restore)
  - Recycle Bin (protects from accidental deletion)
  - Fast Snapshot Restore (costly)

### AMI (Amazon Machine Image)
- Prepackaged software/config/OS.
- Launch EC2 from public, custom, or marketplace AMIs.
- **Creation:** Start EC2 → Customize → Stop → Build AMI → Launch new EC2.

### Instance Store
- Physical hard drive, high I/O, ephemeral (data lost when stopped).
- **Use Case:** Buffer, cache, temp storage.

### EBS Volume Types
- **gp1/gp2 (SSD):** General purpose.
- **io1/io2 Block Express (SSD):** Provisioned IOPS, highest performance.
- **st1 (HDD):** Low cost, frequent access.
- **sc1 (HDD):** Lowest cost, infrequent access.
- **Boot Volumes:** gp and io (SSD) types.

### EBS Multi-Attach
- Attach io1/io2 volumes to up to 16 EC2 instances in the same AZ (all have read/write).

### Amazon EFS (Elastic File System)
- Managed NFS; attach to multiple instances in multiple AZs.
- High availability, scalable, expensive.
- Use cases: Content mgmt, web, data sharing, WordPress.
- Linux AMI only.
- Encryption at rest (KMS), pay-per-use, auto-scaling, security group-based.

### EBS vs EFS vs Instance Store
- Refer to diagrams for comparison.

---

## 7. AWS Fundamentals: ELB + ASG

### Scalability & High Availability
- **Vertical Scaling:** Increase instance size.
- **Horizontal Scaling:** Increase number of instances (elasticity).
- **High Availability:** Use multiple AZs to survive data center loss.
- **Scaling Out/In:** Increase/decrease instances.

### Elastic Load Balancing (ELB)
- **Purpose:** Distributes traffic to multiple servers (EC2, Route 53).
- **Health Checks:** ELB checks /health endpoint for HTTP 200.

#### Load Balancer Types:
- **Classic (CLB):** Layer 4 & 7 (TCP, HTTP/HTTPS)
- **Application (ALB):** Layer 7 (HTTP/HTTPS, WebSocket)
- **Network (NLB):** Layer 4 (TCP/UDP, TLS), static IPs, high performance
- **Gateway (GWLB):** Layer 3, integrates with security appliances

#### OSI Layers Overview
- Layer 1: Physical
- Layer 2: Data Link
- Layer 3: Network (IP)
- Layer 4: Transport (TCP/UDP)
- Layer 5: Session
- Layer 6: Presentation
- Layer 7: Application

#### Security Groups
- **Load Balancer:** Allow inbound HTTP/HTTPS from 0.0.0.0/0.
- **Instances:** Allow inbound only from ELB security group.

### NLB (Network Load Balancer)
- **Elastic IP:** Static public IP.
- **Whitelisting:** Restricts access to specific IPs.
- **Keywords:** Extreme performance, TCP/UDP, static IPs (not in free tier).
- **Target Groups:** EC2, private IPs.
- **NLB in front of ALB:** Combines performance and smart routing.

### GWLB (Gateway Load Balancer)
- Balances traffic across security appliances; works at Layer 3 (GENEVE protocol).
- Inspects/filters traffic before reaching EC2/ALB; supports flow stickiness.

### Sticky Sessions (Session Affinity)
- Routes users to the same backend during a session (login state, shopping cart).
- Achieved via cookies.
- Supported: CLB, ALB (not NLB for HTTP/HTTPS).
- **Cookie Types (ALB):** Application-based, duration-based.

### Cross-Zone Load Balancing
- Distributes traffic across all AZs.
- **ALB:** Enabled by default, free.
- **CLB:** Can be enabled/disabled, free.
- **NLB:** Manual setup, extra charges.

### SSL Certificates
- SSL/TLS encrypt data in transit (not at rest).

### Auto Scaling Groups (ASG)
- **Connection Draining (Deregistration Delay):** Allows in-flight requests to finish before instance removal (default 300s).
- **Integration:** Works with ELB.
- **Features:**
  - Scale in/out automatically based on demand.
  - Health checks replace unhealthy instances.
  - Scaling policies: dynamic, scheduled, target tracking.
  - Launch config/template defines AMI and settings.
  - Desired, min, max capacities.
- **When to Use:** Maintain high availability and scale with traffic.

---

## 8. AWS Fundamentals: RDS + Aurora + ElastiCache

### RDS Read Replicas vs Multi AZ

#### Read Replicas
- Asynchronous, eventually consistent.
- Used for read scaling and cross-region reads.
- Readable, manual promotion for failover.
- Can be cross-AZ (free) or cross-region (not free).

#### Multi-AZ Deployments
- Synchronous, strongly consistent.
- High availability/disaster recovery (automatic failover).
- Standby not readable.
- Always cross-AZ (same region).

#### Key Terms
- Read Replica, Eventually Consistent, Replication Lag
- Multi-AZ, Synchronous, Automatic Failover

### Amazon Aurora
- AWS-managed RDBMS (MySQL/PostgreSQL compatible).
- 5x faster than MySQL, 3x faster than PostgreSQL on RDS.
- Auto-scaling storage (10GB–128TB).
- 6 data copies across 3 AZs (4/6 for writes, 3/6 for reads).
- <10ms replication lag, up to 15 read replicas.
- Failover <30s.
- 1 writer + multiple readers.
- Reader endpoint load balances reads; writer endpoint targets master.
- **Serverless:** Auto-scales DB, pay-per-use.
- **Global DB:** Cross-region replication for DR.
- **Backtrack:** Rewind DB without backup.
- **Built-in:** Backups, patching, monitoring, security.

### Amazon RDS Proxy
- Managed proxy for RDS/Aurora.
- **Features:** Connection pooling, auto-scaling, high availability (Multi-AZ), IAM authentication, secrets manager integration.
- **Benefits:** Reduces DB resource usage, avoids connection overloads, minimizes timeouts.
- **Engines Supported:** MySQL, PostgreSQL, MariaDB, SQL Server, Aurora.
- **Security:** Only accessible within VPC.
- **Use Case:** Lambda with high-frequency, short-lived DB connections.

### Amazon ElastiCache
- Managed Redis & Memcached.
- **Use Case:** Cache frequent queries to reduce DB load.
- **Cache Hit/Miss:** Hit = from cache, Miss = from DB (then cached).
- **Redis:** Multi-AZ, auto-failover, durable, supports read replicas.
- **Memcached:** No replication, sharding, multi-threaded.
- **Benefits:** Improves performance, scales read-heavy workloads, session management.
- **Key Notes:** App code changes required; cache invalidation strategy needed.

#### Cache Strategies
- **Cache Invalidation:** Removes stale data (triggered by updates, TTL, logic).
- **Cache Eviction:** Removes any data to free space (LRU, LFU, FIFO).
- **Write Strategies:**
  - Cache-Aside (Lazy Loading): App checks cache first; on miss, fetches from DB and stores in cache.
  - Write-Through: App writes to both cache and DB.
  - Write-Behind: App writes to cache; cache updates DB later.
  - Cache-Around: Reads go to DB on miss; cache only holds "hot" data.

### Amazon MemoryDB for Redis
- Redis-compatible, durable in-memory DB.
- Intended for durability and ultra-fast performance (>160M requests/sec).
- Multi-AZ transaction log for recovery.
- Scales from GBs to hundreds of TBs.
- Use cases: Web/mobile apps, gaming, media streaming, microservices.

---

## 9. Route 53

### DNS Basics
- DNS translates hostnames to IP addresses.
- **Domain Registrar:** Route 53, GoDaddy, etc.
- **DNS Records:** A, AAAA, CNAME, etc.
- **Zone File:** Contains DNS records.
- **Name Server:** Resolves DNS queries.

#### Example Breakdown:
- `http` → Protocol
- `api.www.example.com` → FQDN
  - `www.example.com` → Subdomain
    - `example.com` → Second-Level Domain
      - `.com` → Top-Level Domain (TLD)

### TTL (Time to Live)
- Low TTL: More Route 53 traffic, higher cost, latest records.
- High TTL: Less traffic, lower cost, possibly outdated records.

---

**End of Notes