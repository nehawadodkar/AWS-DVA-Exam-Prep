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
- Fully managed NoSQL database.
- Key-value and document data model.
- Supports **Provisioned** and **On-Demand** capacity modes.
- Global tables for multi-region replication.
- Transactions supported (ACID).

---

## Capacity Units

### 1. Read Capacity Unit (RCU)
- 1 RCU = 1 strongly consistent read per second for **1 item ≤ 4 KB**.
- Eventually consistent read = **0.5 RCU per read**.
- **Formula:**
  - Strongly consistent: `RCU = ceil(ItemSizeKB / 4) * ReadPerSecond`
  - Eventually consistent: `RCU = ceil(ItemSizeKB / 4) * ReadPerSecond * 0.5`

**Example:**  
- Item = 6 KB, 10 strongly consistent reads/sec  
`RCU = ceil(6/4) * 10 = 2 * 10 = 20`

### 2. Write Capacity Unit (WCU)
- 1 WCU = 1 write per second for **1 item ≤ 1 KB**.
- **Formula:** `WCU = ceil(ItemSizeKB / 1) * WritesPerSecond`

**Example:**  
- Item = 3 KB, 5 writes/sec  
`WCU = ceil(3/1) * 5 = 3 * 5 = 15`

---

## Performance Tips
- Use **Query** instead of Scan whenever possible.
- **Partition key design** is critical for evenly distributed load.
- Avoid hot partitions (high traffic to same partition key).
- DynamoDB Accelerator (DAX) for caching.
- Global Secondary Index (GSI) & Local Secondary Index (LSI) for flexible queries.

---

## Transactions
- DynamoDB supports **ACID transactions**.
- Transactional reads/writes **consume 2x capacity**.
- Use for multi-item or multi-table updates requiring atomicity.

---

## Best Practices
- Use **on-demand mode** if traffic is unpredictable.
- Enable **point-in-time recovery (PITR)** for backup.
- Monitor **ConsumedCapacity** in CloudWatch.
- For **large scan operations**, use **pagination**.
- Consider **S3 export for large datasets**.

---

## Common CLI Commands

- List objects (if used with DynamoDB export or S3 integration):
```bash
aws dynamodb list-tables
aws dynamodb describe-table --table-name MyTable
aws dynamodb scan --table-name MyTable
aws dynamodb query --table-name MyTable --key-condition-expression "PK = :pk" --expression-attribute-values '{":pk":{"S":"123"}}'


