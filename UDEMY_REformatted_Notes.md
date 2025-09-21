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
