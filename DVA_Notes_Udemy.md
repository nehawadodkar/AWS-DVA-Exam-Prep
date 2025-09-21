	
4. IAM & AWS CLI
12. IAM Users & Groups Hands On
IAM is a global service. Does not confine to one region. If you create a user, it is valid globally

1:57
4. IAM & AWS CLI
14. IAM Policies
policy json syntax- version,id, statement (sid,effect,principal,action,resource)

2:04
4. IAM & AWS CLI
16. IAM MFA Overview
MFA - password that you know  + security device that you own

3:13
4. IAM & AWS CLI
16. IAM MFA Overview
MFA devices - Amazon authy, google Authenticator, microsoft Authenticator

3:31
4. IAM & AWS CLI
16. IAM MFA Overview
Universal 2nd factor (U2F) key --- YubiKey by Yubico (3rd party) - physical USBkind of device

4:02
4. IAM & AWS CLI
16. IAM MFA Overview
Hardware Key Fob MFA Device - provided by Gemalto (3rd party) , Hardware Key Fob MFA Device for AWS GovCloud (US) - providedby SecurePassID (3rdparty)

0:26
4. IAM & AWS CLI
17. IAM MFA Hands On
you can edit password policy to impose a custom one (eg: you can set up minimumreguired characters, setup password strength parameters, setup password expiration etc)

1:01
4. IAM & AWS CLI
23. AWS CloudShell
aws cloudhsell - like a shell prompt where you can execute aws cli commands

3:02
4. IAM & AWS CLI
23. AWS CloudShell
you can play with settings such as font sice etc for the cloudshell.

you can also open multiple tabs

0:35
4. IAM & AWS CLI
24. IAM Roles for AWS Services
aws roles -- intended to be used - not by physical people - but by aws services.

1:30
4. IAM & AWS CLI
24. IAM Roles for AWS Services
most common roles :

EC2 instance roles

Lambda Function Roles

Roles for CloudFormation

0:00
4. IAM & AWS CLI
26. IAM Security Tools
IAM Role: An identity granting permissions via IAM policies, assumed by AWS services, users, or accounts.

Assume Role Policy (trust policy): Defines who can assume the role (i.e., trusted entities).

IAM Policy: Defines what the role can do (permissions).

Entities that can assume a role:

AWS services

IAM users

Cross-account users

Federated users

Manual assumption via CLI/SDK

Use Cases:

Lambda Execution Role (e.g., writing logs)

EC2 Role (e.g., accessing S3/DynamoDB)

Cross-Account Role (resource sharing across accounts)

Federated Role (external user access)

Roles: Use temporary credentials, and require Trust Policies to be assumed.

Important: IAM and Trust Policies work together for granting access.

0:31
4. IAM & AWS CLI
26. IAM Security Tools
IAM security tools

1. IAM Credentials Report - account level - report of all users and their creds

2. IAM Access Advisor - user level - shows permissions granted to a user and when those services were last accessed. This is useful in implementing the principle of least privilege. We can remove access to services that are not being used.

1:29
4. IAM & AWS CLI
28. IAM Best Practices
IAM Guidelines and Best practices

Dont use the root account except for AWS account setup

One physical user = One AWS user

Assign users to groups and assign permissions to groups

Create a strong password policy

USe and enforce the use of MFA

Create and use Roles for giving permissions to AWS services

Use Access Keys for Programmatic Access (CLI/SDK)

Audit permissions of your account using IAM Credentials Report and IAM Access Advisot

Never share IAM users and AccessKeys

0:51
4. IAM & AWS CLI
29. Shared Responsibility Model for IAM
Shared responsibility model for IAM



0:46
5. EC2 Fundamentals
32. EC2 Basics
EC2 is not just a single service. IT consists of many services -

Renting virtual machines (EC2)

Storing data on virtual drives (EBS)

Distributing load across machines (ELB)

Scaling the services using an auto scaling group (ASG)

13:15
5. EC2 Fundamentals
33. Create an EC2 Instance with EC2 User Data to have a Website Hands On
if you stop an ec2 instance and then start it, aws night change its public ip address

0:15
5. EC2 Fundamentals
34. EC2 Instance Types Basics
EC2 instance types: https://aws.amazon.com/ec2/instance-types/

1:23
5. EC2 Fundamentals
34. EC2 Instance Types Basics
aws instance naming convention :

eg: m5.xlarge

here's the breakdown:

m: instance class

5: generation (AWS improves over time)

2xlarge: size within the instance class (more the size, more the cpu, memoy etc)

0:02
5. EC2 Fundamentals
34. EC2 Instance Types Basics
EC2 Instance Types
General Purpose (T3, M5): Balanced for most applications.

Compute Optimized (C5, C6g): Ideal for compute-heavy tasks (e.g., data analysis, video rendering).

Memory Optimized (R5, X1e): Best for memory-heavy tasks (e.g., large databases, in-memory caches).

Storage Optimized (I3, D2): Suitable for data-heavy workloads with high I/O.

Accelerated Computing (P3, Inf1): Use when you need GPU or custom chip power for machine learning and AI tasks.

3:22
5. EC2 Fundamentals
35. Security Groups & Classic Ports Overview
Security Groups good to know:

a security group can be attached to multiple instance and an instance can have multiple security groups attached to it

if you change your region or VPC, you need to create a new security group

Lives "outside" the EC2 - if traffic is blocked, the EC2 instance wont see it. -- Security Groups act as a network-level firewall, filtering traffic before it reaches EC2—if blocked, the instance never sees the request, as it’s not handled at the OS level.

Its good to maintain one separate security group for SSH access

If your application is not accessible (time out), then its a security group issue

If your application gives a 'connection refused' error, then its an application error and not a security group error.

By default, all inbound traffic is blocked and all outbound traffic is allowed

5:44
5. EC2 Fundamentals
35. Security Groups & Classic Ports Overview
just look at the diagram at this timestamp

6:07
5. EC2 Fundamentals
35. Security Groups & Classic Ports Overview
Classic ports to know:

22 = SSH - log into a linux instance

3389 = RDP (Remote Desktop protocol) - log into a Windows instance

21 = FTP - upload files into a file share

22 = SFTP (secure FTP) - upload files using SSH

80 = HTTP - access unsecured websites

443 = HTTPS - access secured websites



2:35
5. EC2 Fundamentals
42. EC2 Instance Connect
EC2 instance connect wont work if you dont have an inbound rule that opens up the port 22.

2:09
5. EC2 Fundamentals
43. EC2 Instance Roles Demo
NOTE: Never ever ever ever enter your IAM access keys on your EC2 instace (aws configure).

2:17
5. EC2 Fundamentals
43. EC2 Instance Roles Demo
USE IAM Roles instead

0:17
5. EC2 Fundamentals
44. EC2 Instance Purchasing Options
EC2 instance purchasing options-



On demand instances - short workload, predictable pricing, pay be second

Reserved (1 & 3 years) (pay upfront/partial/no upfront)

reserved instances - long workloads (eg: you know you are going to run your server for 3 years)

convertible reserved instances - long workloads but you want flexibility to change instance type down the line

Savings plans (1 & 3 years) - instead of committing to a particular instance type,  committing to an amount of usage in $$. Long workload

Spot instances - very short workloads, cheap, can use instances (less reliable) - use cases - batch jobs, image processing, workloads with flexible start/end time etc



to be continued....



7:01
5. EC2 Fundamentals
44. EC2 Instance Purchasing Options
EC2 instance purchasing options continued ....



Dedicated Hosts - look an entire physical server - most expensive. use when you have strong compliance requirements etc. can be purchased as on demand or reserved. You can control instance placements.

Dedicated Instances - no other customers will share your hardware. You do not have control over instance placements.

Capacity Reservations - reserve capacity in a specific AZ (availability zone) for any duration. Billing starts immediately, whether used or not. Charged at on-demand rates, but Savings Plans/Reserved Instances may apply.

Must cancel manually to stop charges.

7:52
5. EC2 Fundamentals
44. EC2 Instance Purchasing Options
watch this slide . very good resort anology to understand EC2 purchasing options

1:23
6. EC2 Instance Storage
45. EBS Overview
EBS Volumes (Elastic Block Storage) - like "network usb stick"

you can attach them to your ec2 instanes

can only be mounted to one instances at time (CCP level) - architect level (some are multi attach rarely).

One instance can have multiple ebs volumes attached to it

ebs from one avaiablility zone can not be attached to an ec2 from another instance in another az. you need to snapshot them to move them across.

free tier - 30 gb of free ebs storage or magnetic per month.

You need to specify upfront how many gbs or IOPS while provisioning.Can be extended later.

2:06
6. EC2 Instance Storage
45. EBS Overview
EBSdelete on termination attribute - cheked by default for root volume and unchecked by default for other volumes. However, you can check and uncheck whatever you want. Watch the slide. This can come in the exam.

0:22
6. EC2 Instance Storage
47. EBS Snapshots
what is an EBSsnapshot ? - backup of the volume at a point in time. We can copy snapshots from one AZ to another like this - take a snapsot of one ebs volume, then restore the snapshot into another ebs volume, which can be in another region.

2:04
6. EC2 Instance Storage
47. EBS Snapshots
EBS features -

EBS snapshots archive

75% cheaper

24-72 hours for restoring

Recycle bin

if you specify recycle bin, it protects EBS from accidental deletion by putting them in recycle instead of permanent deletion.

need to specify how long to keeping recycle bin (1 day to 1 year)

fast snapshot restore (FSR)

forceful initialization of snapshot with no latency. Very costly.

1:39
6. EC2 Instance Storage
49. AMI Overview
AMI (Amazon Machine Image)

provides prepackaged  software, configuration, OS, monitoring etc.Launch EC2 instance from AMI.

You can launch your EC2 instance from

Public AMI

Make your own AMI

AWS marketplace AMI

How to create AMI ?

Start EC2 instance ---> Customize it ---> stop it ----> build AMI ----> ( create an new instance from this AMI)



0:01
6. EC2 Instance Storage
51. EC2 Instance Store
EC2 instance store

physical hard drive attached to the ec2 instance.

High I/O performance

ephemeral - when instance is stopped, the data in here gets lost

usecase- buffer,cache, temp storage

1:08
6. EC2 Instance Storage
52. EBS Volume Types
types of EBSvolumes -

gp1/gp2 (SSD) - general purpose ssd (most imp for exam) watch next slide

io 1 /io 2 Block Express (SSD) - provisioned iops highest performance SSD  (also imp for exam)

st 1 (HDD) - low cost for frequently accessed

scl (HDD) - lowest cost less frequently accessed



chaacterized by size/thriughput/IOPS (I/O ops per sec)

gp and io (SSD) can be boot volumes

1:26
6. EC2 Instance Storage
53. EBS Multi-Attach
Multi attach io1/ io2 family



Attach the same EBS volume to multiple EC2 instances in same AZ.

Each instance has r/w permissions

can attach to 16 EC2 instances at a time (remember this for the exam)

0:46
6. EC2 Instance Storage
54. Amazon EFS
EFS

Elastic filesystem

is a managed NFS- managed network file system

can attach to multiple instances across multiple AZ

highly available,  scalable, very expensive thrice as expensive as gp2

use cases - content management, web serving, data sharing, wordpress

only compatible with linux ami's not windows

encryption at rest using KMS

pay per use

auto scaling

connected using security groups

1:16
6. EC2 Instance Storage
56. EFS vs EBS
watch for EBS vs EFS vs instance store

0:27
7. AWS Fundamentals: ELB + ASG
58. High Availability and Scalability
two types of scalability

vertical

means increase size of the instance (eg going from t2 micro to large)

horizontal (also called as elasticity)

increase number of the instances (implies you have distributed systems)

high availability

means running your application on at least 2 data centers (AZ's)

goal is to survive data center loss

scaling out means increase the number of instances

scaling in means you decrease the number of instances

0:01
7. AWS Fundamentals: ELB + ASG
59. Elastic Load Balancing (ELB) Overview
Load balancer -

A Load Balancer is a server that will forward traffic to multipl downstream servers (EC2 instances, route 53 etc).

3:13
7. AWS Fundamentals: ELB + ASG
59. Elastic Load Balancing (ELB) Overview
Health checks - know whether an EC2 instance is working properly or not. It is done by ELB on a port/route like - /health. It it does not return http 200 (ok), then ELB will not send traffic to that EC2 instance

4:53
7. AWS Fundamentals: ELB + ASG
59. Elastic Load Balancing (ELB) Overview
Types of Load Balancers on aws -



Classic - v1 -- HTTP,HTTPS,TCP,SSL (secure TCP)

Application load balancer - v2 - HTTP,HTTPS,websocket

Network load balancer - v2 - TCP,TLS(secure TCP),UDP

Gateway load balancer - Operates at layer 3 (Network layer) - IP protocol







5:05
7. AWS Fundamentals: ELB + ASG
59. Elastic Load Balancing (ELB) Overview
Networking Layers -

Layer 1 - Physical Layer: The actual cables and hardware used for transmission.

Layer 2 - Data Link Layer: Ensures data packets are correctly transferred over the physical layer.

Layer 3 - Network Layer: Routes data between devices across different networks using IP.

Layer 4 - Transport Layer: Makes sure data arrives safely and in order.

Layer 5 - Session Layer: Manages connections and communication sessions between applications.

Layer 6 - Presentation Layer: Translates data into a format that applications can understand.

Layer 7 - Application Layer: The layer where users interact with applications, like web browsers.

5:07
7. AWS Fundamentals: ELB + ASG
59. Elastic Load Balancing (ELB) Overview
Classic Load Balancer (CLB): Operates at Layer 4 (TCP) and Layer 7 (HTTP/HTTPS). It's the older generation of load balancer.

Application Load Balancer (ALB): Operates at Layer 7. Routes traffic based on request content—ideal for web apps. Supports HTTP to HTTPS redirection, multiple apps on the same EC2 (e.g., containers in ECS), and AWS Lambda. Suitable for microservices and on-prem private IPs.

Network Load Balancer (NLB): Operates at Layer 4. Handles TCP/UDP with high performance and low latency. Provides one static IP per AZ and supports Elastic IPs (useful for IP whitelisting).

Gateway Load Balancer (GWLB): Operates at Layer 3. Integrates with security appliances to inspect and secure incoming traffic.

6:02
7. AWS Fundamentals: ELB + ASG
59. Elastic Load Balancing (ELB) Overview
Security Group for Load Balancer:

Allow inbound HTTP (port 80) and HTTPS (port 443) traffic from any source (0.0.0.0/0).

Security Group for Application Instances Behind the Load Balancer (e.g., EC2 instances):

Allow inbound traffic only from the Load Balancer’s security group (source: Security Group ID of the Load Balancer).

Do not allow traffic from the internet or any other sources.





0:59
7. AWS Fundamentals: ELB + ASG
64. Network Load Balancer (NLB)
Elastic IP - An Elastic IP (EIP) is a static public IP address in AWS that remains constant even if you stop or restart your EC2 instance.

What does whitelisting an ip address mean ? -Whitelisting a specific IP means allowing only that designated IP address to access certain resources or services, while blocking all other IPs. This is commonly used in security configurations, such as security groups in AWS.

1:19
7. AWS Fundamentals: ELB + ASG
64. Network Load Balancer (NLB)
Keywords for Network Load Balancer – Extreme Performance, TCP/UDP, Static IPs--------------Not included in aws free tier.

2:28
7. AWS Fundamentals: ELB + ASG
64. Network Load Balancer (NLB)
NLB target groups -

1. EC2 instances

2.private IP addresses.



You can place an NLB in front of an ALB to combine performance and smart routing.

An NLB operates at Layer 4 (Transport Layer) and handles TCP/UDP traffic with high performance and low latency. It supports millions of requests per second and static IPs.

An ALB operates at Layer 7 (Application Layer) and supports advanced routing based on URL, headers, or cookies.

Setup:

NLB receives incoming traffic.

Forwards it to ALB using a TCP target group.

ALB routes requests to backend services based on application logic.

2:54
7. AWS Fundamentals: ELB + ASG
66. Gateway Load Balancer (GWLB)
ateway Load Balancer (GWLB) – Quick Notes
Balances traffic across third-party security appliances (firewalls, IDS, etc.)

Works at Layer 3 using GENEVE protocol

Not for direct user/app traffic

Adds transparent security in the path (client/server don’t know it's there)

Common use: inspect/filter traffic before it reaches EC2 or ALB

Supports flow stickiness

Often used with VPC endpoint services

Helps scale and manage security appliances like other load balancers

2:12
7. AWS Fundamentals: ELB + ASG
67. Elastic Load Balancer - Sticky Sessions
Sticky Sessions (Session Affinity)
Ensures a user is always routed to the same backend server during a session.

Useful for maintaining user-specific data like login state, shopping cart, or progress.

Achieved by sending a cookie from the load balancer with each request.

Supported by:

Classic Load Balancer (CLB)

Application Load Balancer (ALB)

Not supported by Network Load Balancer (NLB) for HTTP/HTTPS (only TCP).

Cookie types (ALB):

Application-based: Managed by the application.

Duration-based: Set for a fixed time by the load balancer.

3:49
7. AWS Fundamentals: ELB + ASG
68. Elastic Load Balancer - Cross Zone Load Balancing
Cross-Zone Load Balancing – DVA Quick Notes

Distributes traffic across all targets in all Availability Zones (AZs), not just within the request’s AZ.

Useful when the number of targets is not equal in each AZ.

Load Balancer Support:

ALB – Enabled by default, free

CLB – Can be enabled or disabled, free

NLB – Must be enabled manually, extra cost applies

Exam Tip:

NLB cross-zone = manual setup + extra charges

ALB cross-zone = automatic + free

1:58
7. AWS Fundamentals: ELB + ASG
69. Elastic Load Balancer - SSL Certificates
SSL and TLS certificates protect data in transit by encrypting information sent between your browser and a website, so no one can spy on it. They don’t protect data at rest, which is data stored on a server or device.

0:00
7. AWS Fundamentals: ELB + ASG
72. Auto Scaling Groups (ASG) Overview
Connection Draining (now called "Deregistration Delay"):

Used with Elastic Load Balancing (ELB).

Allows in-flight requests to complete before an instance is removed from the load balancer.

Prevents users from getting errors during deployments or scale-in.

Default timeout: 300 seconds (5 minutes), can be configured.

Applies to CLB and ALB (for ALB, it's called Deregistration delay).

0:01
7. AWS Fundamentals: ELB + ASG
76. Auto Scaling Groups - Instance Refresh
Auto Scaling Group (ASG) Cheat Sheet (Part 1)
Key Features:
Scale In / Scale Out – Automatically adjust the number of EC2 instances based on demand.

Scaling Policies – Define when and how to scale (e.g., target CPU, network traffic).

Health Checks – Replaces unhealthy instances automatically.

Elastic Load Balancer (ELB) Integration – Integrates with ELB to automatically add/remove EC2 instances from the load balancer.

0:10
7. AWS Fundamentals: ELB + ASG
76. Auto Scaling Groups - Instance Refresh
Auto Scaling Group (ASG) Cheat Sheet (Part 2)
Types of Scaling:
Dynamic Scaling: Adjust based on load (e.g., increase instances during high traffic).

Scheduled Scaling: Set specific times to scale in/out (e.g., scale up every day at 9 AM).

Target Tracking Scaling: Automatically adjust to maintain a specific metric (like CPU usage).

Components of ASG:
Launch Configuration/Template: Specifies the AMI and instance configuration for the instances in the ASG.

Desired Capacity: The number of EC2 instances you want in the ASG.

Minimum & Maximum Capacity: Defines the scale range.

Scaling Policies: Define when scaling actions should occur based on metrics (CPU, memory, etc.).

0:25
7. AWS Fundamentals: ELB + ASG
76. Auto Scaling Groups - Instance Refresh
Auto Scaling Group (ASG) Cheat Sheet (Part 3)


When to Use:
Automatically manage EC2 instance count based on traffic.

Maintain high availability by ensuring that only healthy instances are running.

Scale out during traffic spikes and in when demand decreases.

4:10
8. AWS Fundamentals: RDS + Aurora + ElastiCache
78. RDS Read Replicas vs Multi AZ
RDS read replicas -

RDS Read Replicas are asynchronously replicated, read-only copies of a source RDS instance, used to offload read traffic and improve read scalability. They are eventually consistent and support horizontal scaling of read operations.



Cross AZ read replicas are free... but cross region read replicas are not free.

0:03
8. AWS Fundamentals: RDS + Aurora + ElastiCache
78. RDS Read Replicas vs Multi AZ
1. Asynchronous Replication

- Used by: RDS Read Replicas

- Type: Asynchronous

- Consistency: Eventually consistent

- Purpose: Read scaling, cross-region read support

- Readable: Yes

- Failover: Manual promotion required

- Lag: Possible replication lag

- AZ: Can be cross-AZ / cross-region



2. Synchronous Replication

- Used by: RDS Multi-AZ Deployments

- Type: Synchronous

- Consistency: Strongly consistent

- Purpose: High availability, disaster recovery

- Readable: No (standby not readable)

- Failover: Automatic

- Lag: No lag (real-time replication)

- AZ: Always cross-AZ (within same region)



Key Terms:

- Read Replica, Eventually Consistent, Replication Lag

- Multi-AZ, Synchronous, Automatic Failover

1:04
8. AWS Fundamentals: RDS + Aurora + ElastiCache
80. Amazon Aurora
aurora storage grows automatically

aurora can have upto 15 replicas

aurora has 6 copies if your data in 3 AZ's - 4 copies needed for writes n 3 needed for reads -- cross region replication

aurora master instance takes writes

automated failover to master in less 30 secs !!

it also has read replicas for load balancing

0:00
8. AWS Fundamentals: RDS + Aurora + ElastiCache
80. Amazon Aurora
Aurora - cheat sheet

AWS-managed RDBMS, MySQL & PostgreSQL compatible.
5x faster than MySQL, 3x faster than PostgreSQL on RDS.
Auto-scaling storage: 10GB to 128TB.
6 data copies across 3 AZs (high availability).
4/6 needed for writes, 3/6 for reads.
<10ms replication lag, up to 15 Read Replicas.
Failover in under 30 sec.
1 Writer + multiple Readers.
Reader endpoint load balances reads.
Writer endpoint targets current master.
Serverless: Auto-scales DB, pay-per-use.
Global DB: Cross-region replication for DR.
Backtrack: Rewind DB without backup.
Built-in: Backups, patching, monitoring, security.
Auto-scaling read replicas.
Shared distributed storage backend.
Load balancing at connection level.

0:05
8. AWS Fundamentals: RDS + Aurora + ElastiCache
80. Amazon Aurora
there will be lot of questions about Aurora i n the exam.

4:13
8. AWS Fundamentals: RDS + Aurora + ElastiCache
83. RDS Proxy
Amazon RDS Proxy - Quick Notes

What:
Fully managed database proxy for RDS and Aurora.

Key Features:

Connection pooling – Reduces direct DB connections

Auto-scaling and serverless – Scales automatically

High Availability – Multi-AZ support, faster failover

IAM authentication – Secure access with IAM roles

Secrets Manager – Stores DB credentials securely

No code changes – App connects to proxy instead of DB

Benefits:

Reduces DB resource usage (CPU, memory)

Avoids connection overloads (e.g., with Lambda)

Minimizes timeouts

Supported Engines:
MySQL, PostgreSQL, MariaDB, SQL Server, Aurora MySQL/PostgreSQL

Security:
Only accessible within VPC (not public)

Common Use Case:
Used with AWS Lambda to manage high-frequency, short-lived DB connections efficiently

0:06
8. AWS Fundamentals: RDS + Aurora + ElastiCache
84. ElastiCache Overview
Amazon ElastiCache Cheat Sheet
Service: Managed Redis & Memcached cache.

Use Case: Caches frequent queries to reduce DB load.

Cache Hit: Data from cache.

Cache Miss: Data from DB, then cached.

Redis vs Memcached:
Redis:

Multi-AZ, auto-failover.

Data durability via AOF persistence.

Supports read replicas.

Best for complex data structures.

Memcached:

No replication/high availability.

Uses sharding.

Multi-threaded for better performance.

Benefits:
Improves performance and scales read-heavy workloads.

Session management: Keeps app stateless by storing session data.

Key Notes:
App code changes needed.

Requires cache invalidation strategy.

0:01
8. AWS Fundamentals: RDS + Aurora + ElastiCache
87. Amazon MemoryDB for Redis - Overview
Amazon MemoryDB for Redis:

Redis-compatible in-memory database service.

Unlike Redis, MemoryDB is intended as a durable database, not just a cache.

Provides ultra-fast performance: Over 160 million requests per second.

Durable data storage with Multi-AZ transaction log for recovery and reliability.

Scales from tens of gigabytes to hundreds of terabytes.

Ideal for web/mobile applications, online gaming, media streaming, and microservices.

Provides fast recovery and data durability across Availability Zones (AZs).

0:01
8. AWS Fundamentals: RDS + Aurora + ElastiCache
86. ElastiCache Strategies
Cache Invalidation vs Cache Eviction

Cache Invalidation:

Removes stale or outdated data.

Keeps cache consistent with the source (like a database).

Triggered by data updates, TTL (time-to-live), or app logic.

Used when data changes and needs to be refreshed.

Example: User profile updated → old cache cleared.

Cache Eviction:

Automatically removes items when cache is full.

Ensures performance by freeing up space.

Controlled by cache policies like LRU, LFU, FIFO.

Does not care if data is stale—just makes space.

Example: Least-used item evicted to store new data.

Summary:

Invalidation = removes old data for accuracy.

Eviction = removes any data to make space.

3:51
8. AWS Fundamentals: RDS + Aurora + ElastiCache
86. ElastiCache Strategies
Cache Write Strategies - Cheat Notes

Cache-Aside (Lazy Loading):
App checks cache first. On miss, gets from DB, stores in cache. Good for read-heavy, infrequent writes.

Write-Through:
App writes to cache and DB together. Ensures consistency. Slower writes, fast reads.

Write-Behind (Write-Back):
App writes to cache only; cache updates DB later. Fast writes, possible data loss on failure.

Cache-Around:
Reads go to DB on miss, no cache update. Cache holds only hot data. Good for volatile/infrequent data.

1:17
9. Route 53
91. Route 53 - Creating our first records
TTL - time to live - how long data will stay in the cache.

Low TTL - more traffic on route 53. Means more cost.  Get latest records mostly

High TTL - less traffic on route 53. Means less cost. Outdated records

0:26
9. Route 53
88. What is a DNS?
DNS - translates human friendly hostnames into machine IP addresses.

1:38
9. Route 53
88. What is a DNS?
Domain registrar - route 53, GoDaddy etc

DNS records - A, AAA, CNMAE etc

Zone File: contains DNS records

Name server: Resolves DNS queries

2:51
9. Route 53
88. What is a DNS?
http → Protocol

api.www.example.com → FQDN

├── www.example.com → Subdomain

     ├── example.com → Second-Level Domain

         ├── .com → Top-Level Domain (TLD)

             └── . (last dot) → Root

0:26
9. Route 53
89. Route 53 Overview
Route 53 is an authoritative - customer (YOU) have full control over the DNS. You canupdate the DNS records

1:18
9. Route 53
89. Route 53 Overview
Route 53 is also a domain registrar

4:22
9. Route 53
89. Route 53 Overview
hosted zones - A container for records that define how to route traffic to a domain and its sub domains.

public Hosted Zones - containes records that speify how to route traffic on the internet.

Private Hosted Zones - contain records that specify how you route traffic within one or more VPC.

1:28
9. Route 53
91. Route 53 - Creating our first records
A Record (Address Record)
Purpose: Maps a domain to an IP address (IPv4).

Example: example.com → 192.0.2.1

Use: Directs web traffic to the IP address of the server.

CNAME Record (Canonical Name Record)
Purpose: Creates an alias for another domain.

Example: www.example.com → example.com

Use: Makes www.example.com point to the same server as example.com.

MX Record (Mail Exchange Record)
Purpose: Defines mail servers for receiving emails for the domain.

Example: example.com → mail.example.com

Use: Routes email to the correct mail server for example.com.

1:13
9. Route 53
94. Route 53 CNAME vs Alias
CNAME:

Points a hostname to any other hostname. (app.mydomain.com ---> blabla.anything.com)

Only for  non root domain (aka.something.mydomain.com)

Alias:

Points a hostname to an AWS Resource (app.mydomain.com -->blabla.amazonaws.com)

works for root and nor root domain

free

have native health check facility available

2:48
9. Route 53
94. Route 53 CNAME vs Alias
Alias can be set for :

Elastic Load Balancer, CloudFront, API Gateway, Elastic Bean Stalk, S3 Websites, VPC Interface Endpoints, Global Accelerator, Route 53 Record

2:55
9. Route 53
94. Route 53 CNAME vs Alias
you can not set an Alias for an EC2 DNS name.

0:10
9. Route 53
98. Route 53 Health Checks
Route 53 Routing Policies

Simple: One record, one resource, no health checks.

Weighted: Split traffic by weight (percentages).

Latency-based: Route to lowest latency server. (mostly nearest server)

Failover: Primary and backup, switch if health check fails. If primary fails, route to secondary.

Geolocation: Route based on user’s country or location. (route french sources to french version of my app etc)

Geoproximity: Route based on location + bias (needs Traffic Flow).

Multi-Value Answer: Return multiple healthy IPs, basic health checks.

0:01
9. Route 53
98. Route 53 Health Checks
Route 53 Health Checks
Purpose: Check health of public endpoints (and private resources using CloudWatch alarms).

Public Health Check: Monitors endpoint (HTTP, HTTPS, TCP); 15 AWS global checkers; healthy if >18% pass; interval 30s (normal) or 10s (fast).

Calculated Health Check: Combines results of multiple health checks (AND/OR/NOT logic); up to 256 child checks.

Private Resource Health Check: Use CloudWatch Metrics + Alarm → tie alarm to Route 53 health check (for VPC/private endpoints).

Failover: If health check fails, DNS automatically redirects traffic to healthy endpoints.

0:00
9. Route 53
106. 3rd Party Domains & Route 53
3rd Party Domains & Route 53
You can register a domain with any domain registrar (e.g., GoDaddy, Google Domains) and still use Route 53 for DNS management. After registering the domain, create a public hosted zone in Route 53 and update the name servers (NS) at the registrar to Route 53’s name servers. This allows Route 53 to manage your domain’s DNS records while the registrar manages domain ownership.

Registrar: Manages domain ownership.

DNS Service: Manages routing and DNS records.

2:31
11. Amazon S3 Introduction
114. S3 Overview
S3 is NOT a global service. But bucket should be globally unique.

2:49
11. Amazon S3 Introduction
114. S3 Overview
S3 bucket naming conventions -

No uppercase, No underscore

3-63 characters long

Not an IP

Must start with lowecase letter or a number

Must NOT start with the prefix xn--

Must NOT end with the suffix -s3alias

0:00
11. Amazon S3 Introduction
115. S3 Hands On
S3 object key means --> full path + file name.

eg: folder1/folder2/file1.txt

1:23
11. Amazon S3 Introduction
116. S3 Security: Bucket Policy
S3 Bucket policies -

1. user based - IAM

2. Resource based

Bucket policies

Object ACL

Bucket ACL



1:18
11. Amazon S3 Introduction
122. S3 Replication
S3 bucket replication

Cross region replication

Same region replication

Buckets can be in diff AWS accounts

Copyig is asychronous

Must give proper IAM permissions to S3

0:24
11. Amazon S3 Introduction
123. S3 Replication Notes
S3 replication is only for new objects to be replicated.

If you want to replicate existing objects, u need to use S3 Batch Replication

0:53
11. Amazon S3 Introduction
123. S3 Replication Notes
Delete markers can be replicated using an optional setting

But permanent deletes can not be replicated

"Chaining" replication is NOT ALLOWED.Eg: replicating from bucket 1 to bucket 2 and then  to bucket 3.

0:28
11. Amazon S3 Introduction
124. S3 Replication - Hands On
replication only works if versioning is enabled.

0:21
11. Amazon S3 Introduction
125. S3 Storage Classes Overview
Amazon S3 Storage Classes:

Amazon S3 Standard - General Purpose

Amazon S3 Standard - Infrequent Access (IA)

Amazon S3 One zone - Infrequent Access

Amazon S3 Glacier Instant Retrival

Amazon S3 Glacier Flexibal Retrival

Amazon S3 Glacier Deep Archive

Amazon S3 intelligent Tiering



0:36
11. Amazon S3 Introduction
125. S3 Storage Classes Overview
Can move between classes manually or using S3 lifecycle configurations

1:38
11. Amazon S3 Introduction
125. S3 Storage Classes Overview
S3 durability - 11 9's

S3 standard availability - 99.99%

5:29
11. Amazon S3 Introduction
125. S3 Storage Classes Overview
look at the table at this timestamo

1:16
12. AWS CLI, SDK, IAM Roles & Policies
127. AWS EC2 Instance Metadata
IMDS - EC2 instance metadata -EC2 instances can learn about themselves w/o using an IAM role for that purpose. there an IMDSv1 and IMDSv2 - IMDS v1 can directly access the metadata url whereas v2 needs to generate the session token and then use it in header.

2:42
12. AWS CLI, SDK, IAM Roles & Policies
129. AWS CLI Profiles
AWS profiles - Managing/configuring multiple aws accounts for the cli activities.

0:41
12. AWS CLI, SDK, IAM Roles & Policies
130. AWS CLI with MFA
MFA with CLI: 

To use MFS with CLI, you should create a temp sesson. To do so, you must run the "STS GetSessionToken" api call . in that call you wikl need to pass the arn of the mfa device, the mfa token and duration for which you want to use the creds. It will give you back the keys and token needed.

0:47
12. AWS CLI, SDK, IAM Roles & Policies
131. AWS SDK Overview
SDK -  there are official SDK's available for multiple languages. rg: boto3 for python. SDK's are used when we want to call aws commands etc from application programs. such as lambda functions or any other code anywhere like github, local machine etc

1:13
12. AWS CLI, SDK, IAM Roles & Policies
132. Exponential Backoff & Service Limit Increase
AWS API Rate limits - How many aws api calls can you do can you do for a particular aws service in a given amount of time (eg: how many s3 api calls can you do per second). There are fixed api rate limits for each amazon service.



Service Quotas (Service Limits) - How many instance of a service can you run at a time. eg: how many on demand stand instances can you spin at a time. AWS has a threshold on that. If you want it increased, you need to raise a ticket or use the service quotas api.

1:57
12. AWS CLI, SDK, IAM Roles & Policies
132. Exponential Backoff & Service Limit Increase
If you do too many api calls (outside your assigned limit), you will get a "ThrottlingException". If you get this exception frequently, you should use "Exponential Backoff". It implements retry logic. you should only implement retry logic on the 5xx server errors (eg: 503) and throttling.  Never implement retry logic on 4xx errors (eg: 403)The retry mechanism is included in the SDK api calls. But if you are using the aws api's as is, then , you will need to handle this retry logic yourself.



simple example: lets say you got error because you are trying to make way too many api calls at a time and the server is out of capacity. just use a retry logic by exponentially increasing the weight times with each retry. This way there's a good chance that eventually, the server will be free enough to cater to you.



1:09
12. AWS CLI, SDK, IAM Roles & Policies
133. AWS Credentials Provider & Chain
priority in which the creds will be looked upon by aws is ---

command line

Environment variables

CLI creds file

CLI conf file

Container creds

Instance profile creds

2:16
12. AWS CLI, SDK, IAM Roles & Policies
133. AWS Credentials Provider & Chain
check out the example on this slide

0:03
13. Advanced Amazon S3
135. S3 Lifecycle Rules (with S3 Analytics)
The URL to access EC2 instance metadata from within the instance is: http://169.254.169.254/latest/meta-data/ -- you can curl to it.

1:04
13. Advanced Amazon S3
135. S3 Lifecycle Rules (with S3 Analytics)
S3 Lifecycle Rules - Configure object transition to other storage classes.

Expiration Actions- Configure objects to expire(delete) after some time.

1:14
13. Advanced Amazon S3
139. S3 Performance
S3 baseline performance - 35000 put/copy/paste/delete or 55000 get/head per second per prefix in a bucket.

3:12
13. Advanced Amazon S3
139. S3 Performance
Multi part upload - divide large files in parts and upload those parts & upload those parts parallelly.

S3 Transfer Acceleration - To increase transfer speed by transferring file to an AWS edge location (using internet), which will forward data to the S3 bucket in the target region (using private AWs network).

4:24
13. Advanced Amazon S3
139. S3 Performance
S3 Byte range fetches - speeding up the reads from S3 by dividing large files into byte ranges and getting those byte ranges in parallel. You can also retrieve partial data from the file if thats all you need. Eg: if you need only header.

0:47
13. Advanced Amazon S3
140. S3 Object Tags & Metadata
you can define user defined metadata to S3 objects as well, in addition to existing system defined metadata. the user defined metadata should always start with "x-amz-meta-"

1:25
13. Advanced Amazon S3
140. S3 Object Tags & Metadata
you can use object tags for S3 objects as well. They are in  the form of key value pairs. They can be used for assigning fine grained permissions to objects.

0:01
14. Amazon S3 Security
141. S3 Encryption
S3 Encryption Summary

SSE-S3 – AWS manages keys.
Header: x-amz-server-side-encryption: AES256.
Default for new buckets/objects. No key access.

SSE-KMS – Keys via AWS KMS.
Header: x-amz-server-side-encryption: aws:kms.
More control. Need access to both object and KMS key.
Usage is logged in CloudTrail. API calls count toward KMS quotas.

SSE-C – Customer provides key (not stored by AWS).
Must use HTTPS. Key sent in headers each time.
Same key required to retrieve object.

Client-Side Encryption –
You encrypt before uploading using AWS SDK.
You manage keys. S3 stores encrypted data as-is.

Encryption in Transit –
Use HTTPS for secure transfer.
SSE-C requires HTTPS.
To enforce HTTPS, deny access in bucket policy if aws:SecureTransport is false.

4:19
14. Amazon S3 Security
145. S3 CORS
CORS Cheat Sheet (Cross-Origin Resource Sharing)

Browser security feature that controls cross-origin requests

Origin = protocol + domain + port

Cross-origin requests are blocked unless explicitly allowed

Controlled using the Access-Control-Allow-Origin header

Server can allow specific origins or use * to allow all

Preflight request (OPTIONS) checks allowed methods and headers

Server must respond with allowed origins and methods

Required in S3 when a static website loads assets from another bucket

Target S3 bucket must have proper CORS configuration

CORS applies only in browsers (not backend services)

Common exam topic: how to enable cross-origin access in S3 static websites

To fix cross-origin errors, configure CORS rules in the S3 bucket serving the assets

1:14
14. Amazon S3 Security
147. S3 MFA Delete
MFA delete forces user to use MFA to permanently delete an object version or disable versioning. Versioning must be enabled in order to enable MFA delete. Only root user ca enable/disable MFS delete.

0:01
14. Amazon S3 Security
149. S3 Access Logs
S3 access logs - logging all access attempts to the S3 applications in a separate bucket which can be used for data analysis using tools such as Athena.

0:03
14. Amazon S3 Security
147. S3 MFA Delete
An S3 pre-signed URL is a secure, time-limited link that allows users to upload or download a specific S3 object without needing direct AWS credentials.



use cases :

temporary access to one particular file without making it public.



eg:

A user can upload a file to your S3 bucket using a pre-signed URL without needing AWS access.

You can share a secure download link for a private S3 object that expires after a set time.

An application can temporarily allow access to S3 files for processing by another system or service.

0:04
14. Amazon S3 Security
151. S3 Pre-signed URLs
Amazon S3 Access Points are customizable entry points to a bucket that simplify managing access for different applications, users, or teams with distinct permissions and network controls.

0:13
15. CloudFront
155. CloudFront - Overview
Cloudfront -

Content Delivery Network (CDN)

- caches contents at egde locations

- reduces latency

-DD0S attach prevention

Cloudfront Origins - Backends you want to connect your cloudfront to.

5:03
15. CloudFront
155. CloudFront - Overview
Cloudfront VS Cross-region replication (CRR)



S3 + CloudFront

Uses global CloudFront edge locations to cache and deliver your S3 content faster

Reduces latency and offloads requests from the main S3 bucket by serving cached files

Use case: Speed up delivery of static websites, images, or videos to users worldwide

Key difference: Optimizes content delivery speed but does NOT replicate or back up data

S3 Cross-Region Replication (CRR)

Automatically copies S3 objects from one AWS region to another to keep data synchronized

Helps with disaster recovery, compliance, and data durability across regions

Use case: Maintain a backup of your data in a different AWS region for safety

Key difference: Provides data replication and backup but does NOT improve delivery speed

5:53
15. CloudFront
157. CloudFront - Caching & Caching Policies
CloudFront Caching - Summary

Each edge location has its own cache.

Cache Key = hostname + URL path by default.

On request: CloudFront checks cache → if missing, fetches from origin and caches it.

Goal: Maximize cache hit ratio.

Cache Key Customization

Can include headers, cookies, or query strings.

Controlled via Cache Policy (none, whitelist, all, etc.).

Invalidation

Removes cached items before TTL expires.

Origin Request Policy

Forwards extra headers/cookies/query strings to origin without adding them to cache key.

Use when origin needs info not used in caching.

Key Diff

Cache Policy = controls what defines cache key.

Origin Request Policy = controls what gets sent to origin.

1:10
15. CloudFront
163. CloudFront Signed URL / Cookies
CloudFront Signed URL / Cookie

Used to give secure, temporary access to private content via CloudFront

Signed URL = access to one file

Signed Cookie = access to multiple files

Can restrict access by IP, path, time, etc.

Best when S3 is behind CloudFront and access is private

Requires CloudFront key pair for signing

S3 Pre-signed URL

Gives temporary direct access to an S3 object using IAM permissions

Bypasses CloudFront and goes straight to S3

Limited time access, no caching

Best for short-term S3 access without CloudFront

When to use:

Use CloudFront Signed URL when your content is behind CloudFront

Use S3 Pre-signed URL for direct, short-term S3 access

4:15
16. ECS, ECR & Fargate - Docker in AWS
167. Docker Introduction
Amazon ECR - Amazon elastic container repository. - its like docker hub

4:47
16. ECS, ECR & Fargate - Docker in AWS
167. Docker Introduction
Amazon ECS - Elastic container service - Amazon's own container platform

Amazon EKS - Elastic Kubernetes service - Amazon's managed Kubernetes (open source)

AWS Fargate - Amazon's own serverless container platform -  Works with ECS and EKS

Amazon ECR - Used to store container images (Its like DockerHub)

2:15
16. ECS, ECR & Fargate - Docker in AWS
168. Amazon ECS
The EC2 launch type and Fargate launch type are two ways to run containers in Amazon ECS (Elastic Container Service). Here's a clear comparison:

EC2 Launch Type
You manage the infrastructure.
You launch a cluster of EC2 instances and run containers on them.
You are responsible for provisioning, patching, and scaling EC2 instances.
You can customize instance size, type, and OS.
Best when you need full control, want to run background services, or optimize costs with EC2 Spot Instances.

Fargate Launch Type
AWS manages the infrastructure.
You only define container specs (CPU, memory, image); AWS runs it.
No need to manage EC2 instances.
You pay only for the CPU and memory used.
Ideal for simplified deployments, small teams, and serverless-style container apps.

0:00
16. ECS, ECR & Fargate - Docker in AWS
172. Amazon ECS - Auto Scaling
ECS auto scaling uses AWS Application Auto Scaling

Can scale on CPU Utilization, Memory Utilization, ALB Request Count per Target

Scaling types are Target Tracking (preferred), Step Scaling, Scheduled Scaling

Fargate is easier to scale because it is serverless

EC2 launch type requires scaling EC2 instances too

EC2 scaling options:

ASG scaling based on metrics

ECS Capacity Provider (preferred) which auto scales ASG when tasks lack capacity

Example:

CPU increases, CloudWatch alarm triggers

Desired task count increases

New task launches

If EC2 is used, Capacity Provider adds EC2 instances

Exam tip: prefer Fargate or Capacity Provider for EC2 launch type

2:32
16. ECS, ECR & Fargate - Docker in AWS
173. Amazon ECS - Rolling Updates
Rolling Update means updating an application gradually.

Old versions are replaced by new versions one batch at a time.

This keeps the app running without downtime.

It updates some tasks/instances, waits for them to be healthy, then updates the next batch.

Used in ECS, Kubernetes, and many deployment tools.

0:34
16. ECS, ECR & Fargate - Docker in AWS
175. Amazon ECS Task Definitions - Deep Dive
ECS Task definitions are metadata in JSON form to tell ECS how to run a Docker container.

8:59
16. ECS, ECR & Fargate - Docker in AWS
175. Amazon ECS Task Definitions - Deep Dive
ECS task definition tells ECS how to run containers.

Key fields: image, CPU, memory, container/host ports, env vars, IAM task role, logging.

In EC2, define container and host ports. Host port 0 = dynamic; ALB maps it.

In Fargate, only container port needed; each task gets its own IP.

IAM role is set in the task definition (not service) and gives tasks AWS access.

Env vars can be hardcoded or pulled from SSM/Secrets Manager.

You can define up to 10 containers per task. Use bind mounts to share data between containers.

EC2 uses instance storage for volumes; Fargate uses ephemeral (20–200 GB).

ALB works with dynamic ports in EC2 and fixed ports in Fargate.

5:51
16. ECS, ECR & Fargate - Docker in AWS
177. Amazon ECS - Task Placements
ECS Task Placement – DVA Exam

Applies only to EC2 launch type, not Fargate.

ECS picks EC2 instance based on CPU, memory, ports, then applies placement constraints and strategy.

Placement Strategies:

binpack – Packs tasks on instance with least available CPU/memory (cost-saving).

spread – Evenly spreads tasks (e.g., across AZs or instance IDs).

random – Places tasks randomly.

Placement Constraints:

distinctInstance – One task per EC2 instance.

memberOf – Use cluster query (e.g., only t2 instance type).

3:58
16. ECS, ECR & Fargate - Docker in AWS
183. Amazon EKS
Amazon EKS – DVA Exam

EKS = Elastic Kubernetes Service; runs Kubernetes on AWS.

Alternative to ECS; EKS uses Kubernetes API (open-source, cloud-agnostic).

Supports EC2 (self-managed or managed node groups) & Fargate launch modes.

Use case: migrating Kubernetes workloads, or already using Kubernetes elsewhere.

Kubernetes tasks = Pods; run on EKS Worker Nodes (EC2).

EKS can use ALB/NLB for public/private access.

Node Types:

Managed Node Group: AWS creates/maintains EC2 nodes in ASG.

Self-managed: You control EC2 nodes; must register with cluster.

Fargate: Serverless, no node management.

Storage (via CSI driver):

EC2: EBS, EFS, FSx.

Fargate: Only EFS is supported.

2:32
16. ECS, ECR & Fargate - Docker in AWS
174. Amazon ECS - Solutions Architectures
watch this slide about solution architectures

4:49
16. ECS, ECR & Fargate - Docker in AWS
175. Amazon ECS Task Definitions - Deep Dive
watch the task definitions - deep dive slide as well.

0:01
16. ECS, ECR & Fargate - Docker in AWS
175. Amazon ECS Task Definitions - Deep Dive
Task definition tells ECS how to run containers. Made in JSON or through console UI.

Includes: image name, ports, CPU/memory, environment variables, IAM role, logging.

EC2 launch type: needs container and host port. Host port can be 0 for dynamic mapping. ALB handles routing. Security group must allow all ports from ALB.

Fargate launch type: only container port needed. Each task gets a private IP. Security group allows port 80 or 443 from ALB.

IAM role: defined in task definition, used by all tasks from it.

Environment variables: can be hardcoded or pulled from SSM/Secrets Manager. Can also load from S3 file.

Data sharing between containers: use bind mount. On EC2 it's tied to the instance. On Fargate it's ephemeral storage (20–200 GB).

5:54
16. ECS, ECR & Fargate - Docker in AWS
177. Amazon ECS - Task Placements
ECS Task Placement (EC2 only)
Used to decide where to place tasks in an ECS cluster (based on CPU, memory, ports).

Placement Process

Step 1: Check CPU, memory, port requirements

Step 2: Apply placement constraints

Step 3: Apply placement strategy

Placement Strategies

binpack – Packs tasks tightly on least available CPU/memory (cost-effective)

random – Places tasks randomly

spread – Spreads tasks across AZs or instance IDs (high availability)

Strategies can be combined

Placement Constraints

distinctInstance – Each task on a different EC2 instance

memberOf – Use expressions/queries (e.g., only on t2 instance types)

0:00
16. ECS, ECR & Fargate - Docker in AWS
181. AWS CoPilot - Overview
AWS Copilot (CLI Tool)
Not a service – it's a CLI tool

Used to build, release, operate container apps

Deploys to App Runner, ECS, or Fargate

Hides infra complexity (VPC, ECS, ELB, ECR, etc.)

Use CLI or YAML to define microservice architecture

Supports multi-environment deployment

Integrates with CodePipeline (CI/CD)

Includes logging, health checks, troubleshooting

Creates right-sized, auto-scaling infra

Helps you focus on app code, not infra setup

3:56
16. ECS, ECR & Fargate - Docker in AWS
183. Amazon EKS
EKS Launch Types:

EC2: You manage worker nodes

Fargate: Fully serverless, no node management

Why use EKS: If you're already using Kubernetes elsewhere or want standard API

Node Types:

Managed: AWS manages EC2 in Auto Scaling Group

Self-managed: You manage EC2 nodes and register to EKS

Fargate: No EC2 nodes at all

Pods = Kubernetes units (like ECS tasks)

Storage options via CSI driver:

EBS

EFS (only supported in Fargate)

FSx for Lustre / NetApp ONTAP

Load balancers: Public or private supported for services

0:00
17. AWS Elastic Beanstalk
188. Beanstalk Deployment Modes
Elastic Bean Stalk

1. Deployment Options
All at once: Fastest, downtime, no extra cost. Good for dev.

Rolling: Update batches of instances, runs below capacity, no extra cost.

Rolling with additional batch: New instances added to keep full capacity, small extra cost.

Immutable: Deploy new instances in a temporary Auto Scaling group (ASG), zero downtime, higher cost, fastest rollback.

Blue/Green: Create a new environment, test, then swap URLs. Manual, zero downtime.

Traffic Splitting (Canary Testing): Split traffic between old and new version via ALB, automated rollback, zero downtime, doubles capacity temporarily.

0:01
17. AWS Elastic Beanstalk
191. Beanstalk Lifecycle Policy Overview + Hands On
Elastic Bean Stalk

2. Lifecycle Policy
Max 1000 app versions per account.

Set lifecycle policy to delete old versions by age or count.

Option to keep or delete source bundles in S3.

Elastic Beanstalk service role manages deletion.

0:03
17. AWS Elastic Beanstalk
192. Beanstalk Extensions
Elastic Bean Stalk

3. EB Extensions (.ebextensions)
Directory: .ebextensions/ at root of source code.

Files in YAML or JSON format with .config extension (e.g., logging.config).

Used to set environment variables, modify defaults, add AWS resources (RDS, ElastiCache, DynamoDB).

Resources created via extensions are deleted if the environment is deleted.

Example: setting environment variables via option_settings.

0:01
17. AWS Elastic Beanstalk
193. Beanstalk & CloudFormation
Elastic Bean Stalk

4. CloudFormation Integration
Elastic Beanstalk uses CloudFormation stacks under the hood.

CloudFormation provisions ASG, Load Balancers, Security Groups, scaling policies, etc.

You can extend Beanstalk by provisioning additional AWS resources via .ebextensions using CloudFormation syntax.

0:01
17. AWS Elastic Beanstalk
194. Beanstalk Cloning
Elastic Bean Stalk

5. Cloning Environment
Clone environment to create exact copy (config + resources).

Good for testing with prod config.

RDS data is NOT cloned, only config.

After cloning, customize settings as needed.

0:02
17. AWS Elastic Beanstalk
195. Beanstalk Migrations
Elastic Bean Stalk

6. Migration
Load Balancer Migration:

Cannot change ELB type on existing env.

Create new environment with desired ELB type.

Deploy app, then swap traffic (CNAME or Route 53).

RDS Migration:

For production, separate RDS from Beanstalk.

Snapshot and enable deletion protection on RDS.

Create new env without RDS, point app to existing RDS.

Shift traffic, terminate old env.

Manually delete old CloudFormation stack if stuck.

0:00
18. AWS CloudFormation
214. CloudFormation - StackSets
CloudFormation Cheat Sheet 1

1. Stacks
Collection of AWS resources managed as a single unit.

Create, update, delete stacks via templates.

2. Templates
JSON/YAML file defining resources, parameters, outputs, etc.

Mandatory section: Resources (AWS components).

Use !Ref to reference parameters/resources.

3. Resources
AWS components (e.g., EC2, S3).

Resource type format: AWS::Service::ResourceType.

Use documentation to find properties.

0:00
18. AWS CloudFormation
202. CloudFormation - Parameters
CloudFormation Cheat Sheet 2

4. Parameters
User inputs to customize templates.

Types: String, Number, CommaDelimitedList, AWS-specific, SSM Parameter.

Supports validation: AllowedValues, AllowedPattern, Min/Max, NoEcho (hide sensitive info).

Reference with !Ref ParameterName.

5. Outputs
Optional section to export values (e.g., VPC ID).

Exported outputs can be imported into other stacks with Fn::ImportValue.

Enables stack-to-stack communication.

6. Conditions
Control resource/output creation based on conditions (e.g., environment type).

Use functions: Fn::Equals, Fn::And, Fn::Or, Fn::Not.

Apply with Condition: ConditionName on resources or outputs.

0:00
18. AWS CloudFormation
210. CloudFormation - Deletion Policy
CloudFormation Cheat Sheet 3

7. DeletionPolicy
Controls what happens on resource/stack deletion.

Types:

Delete (default) – resource is deleted.

Retain – resource preserved.

Snapshot – snapshot created before deletion (for DBs, EBS, etc.).

S3 bucket delete fails if not empty.

8. Stack Policies
JSON policy controlling update actions on resources.

Protect critical resources from accidental updates.

Default denies updates unless explicitly allowed.

9. Termination Protection
Prevents accidental stack deletion.

Must disable before deleting the stack.

10. Service Roles
IAM role dedicated to CloudFormation.

Allows CloudFormation to create/update/delete resources using role permissions.

Users need iam:PassRole permission to delegate role.

0:00
18. AWS CloudFormation
213. CloudFormation - Custom Resources
CloudFormationCheat Sheet 4

11. Custom Resources
Extend CloudFormation with Lambda-backed custom logic.

Useful for unsupported resources or custom provisioning (e.g., emptying S3 bucket before delete).

Defined with Type: Custom::ResourceName and ServiceToken (Lambda ARN).

12. StackSets
Deploy/update/delete stacks across multiple accounts and regions.

Managed from an administrator account.

Commonly used with AWS Organizations.

Changes applied simultaneously to all stack instances.

0:30
18. AWS CloudFormation
206. CloudFormation - Intrinsic Functions
CloudFormation Cheat Sheet

10. Intrinsic Functions
!Ref: reference parameter value or resource physical ID

!GetAtt: get resource attribute (e.g., AZ, PublicIp)

!FindInMap: get value from mapping

!ImportValue: import exported value from another stack

!Base64: encode string in Base64 (e.g., UserData)

Condition functions: !If, !Equals, !And, !Or, !Not

Others: !Join, !Sub, !Select, !Split, !Cidr



0:00
18. AWS CloudFormation
207. CloudFormation - Rollbacks
11. Rollbacks
Default: on failure, stack creation/update rolls back (deletes changes)

Option to disable rollback to preserve resources for troubleshooting

Update failures roll back to last known good state

On rollback failure, fix manually then call ContinueUpdateRollback to recover

Option to preserve successfully created resources on failure during creation/update

12. Capabilities
CAPABILITY_IAM: acknowledge templates creating IAM resources without names

CAPABILITY_NAMED_IAM: for IAM resources with explicit names

CAPABILITY_AUTO_EXPAND: for templates with macros or nested stacks

Must be acknowledged in console or CLI (--capabilities)

Missing acknowledgment causes InsufficientCapabilitiesException

2:55
19. AWS Integration & Messaging: SQS, SNS & Kinesis
217. Amazon SQS - Standard Queues Overview
SQS - when you hear decoupling in exam, think about SQS

- unlimited number of messages in the queue

- low latency

- default retention of messages - 4 to 14 days



0:00
19. AWS Integration & Messaging: SQS, SNS & Kinesis
217. Amazon SQS - Standard Queues Overview
Amazon SQS (Simple Queue Service)

Fully managed message queue to decouple apps and enable async processing.

Standard Queue

Unlimited throughput and message volume.

At-least-once delivery (may have duplicates).

Best-effort ordering (messages can arrive out of order).

Max message size: 256 KB.

Retention: 1 min to 14 days (default 4 days).

Low latency (<10 ms).

How it works

Producers send messages (SendMessage API).

Consumers poll, process, then delete messages (DeleteMessage API).

Multiple producers and consumers supported.

Scaling

Use multiple consumers; scale with EC2 Auto Scaling.

Trigger scaling on CloudWatch metric ApproximateNumberOfMessages.

Security

In-flight: HTTPS.

At-rest: KMS encryption.

Access control via IAM and SQS policies.

0:00
19. AWS Integration & Messaging: SQS, SNS & Kinesis
219. SQS Queue Access Policy
SQS Queue Access Policies
What: Resource-based JSON policies attached to SQS queues (like S3 bucket policies).

Use cases:

Allow cross-account access (e.g., EC2 in another account reading the queue).

Allow AWS services (e.g., S3) to send messages to the queue.

How: Policies specify who (principal) can do what (actions like SendMessage) with conditions (source ARN/account).

Why: Needed for cross-account or service-to-queue permissions; without it, access is denied.

Exam tip: Know difference between IAM user policies and queue access policies; queue policies control resource-level permissions.

0:00
19. AWS Integration & Messaging: SQS, SNS & Kinesis
220. SQS - Message Visibility Timeout
message visibility timeout

Message becomes invisible to others for visibility timeout (default 30s) after being received.

If not deleted in time, it becomes visible again (possible duplicate).

Use ChangeMessageVisibility API to extend timeout if more processing time is needed.

Set timeout balanced: too high delays retries, too low causes duplicates.

0:01
19. AWS Integration & Messaging: SQS, SNS & Kinesis
221. SQS - Dead Letter Queues
Dead Letter Queues

DLQ stores messages that fail processing after MaximumReceives attempts.

Used for debugging and preventing endless reprocessing loops.

DLQ type must match source queue type (FIFO ↔ FIFO, Standard ↔ Standard).

Set long retention (e.g., 14 days) and optionally use redrive to source to reprocess after fixing issues.

2:23
19. AWS Integration & Messaging: SQS, SNS & Kinesis
223. SQS - Delay Queues
Delay Queue:

Delay queues postpone message visibility for up to 15 min.

Delay can be set at queue level (default for all messages) or per message (DelaySeconds).

Default = 0 sec (message available immediately).

Useful for postponing processing until a set time after send.

Usecase: Schedule order processing to start 5 minutes after order placement to allow for cancellation changes before fulfillment begins.

0:08
19. AWS Integration & Messaging: SQS, SNS & Kinesis
224. SQS - Certified Developer concepts
1. Long Polling
Definition: Consumer waits (1–20s) for messages if queue is empty, instead of making frequent requests.

Benefits:

Fewer API calls → lower cost, less CPU usage.

Reduced latency → message delivered immediately when available.

Enable:

Queue level: ReceiveMessageWaitTimeSeconds (default 0).

API call level: Pass parameter in ReceiveMessage.

Exam tip: If consumer is doing too many calls & increasing costs → use long polling (prefer 20s).

Long Polling Use case → “Chat app”: User A sends a message, User B’s app is already waiting (long polling), so it pops up instantly without constant polling spam.

0:52
19. AWS Integration & Messaging: SQS, SNS & Kinesis
224. SQS - Certified Developer concepts
2. SQS Extended Client (Large Messages)
Limit: SQS message size ≤ 256 KB.

Solution: Store large payload (e.g., video) in S3, send only pointer metadata in SQS.

Consumer: Reads metadata, fetches full object from S3.

Use case: Large file processing without exceeding SQS size limits.

SQS Extended Client → “Video upload pipeline”: Video uploaded to S3, SQS only sends the S3 link → avoids 256 KB limit and keeps queue lightweight.

3:58
19. AWS Integration & Messaging: SQS, SNS & Kinesis
224. SQS - Certified Developer concepts
3. Key API Calls
CreateQueue: Optionally set MessageRetentionPeriod.

DeleteQueue: Removes queue & all messages.

PurgeQueue: Deletes all messages (queue remains).

SendMessage: Add message (optionally DelaySeconds).

ReceiveMessage: Get 1–10 messages (set MaxNumberOfMessages).

DeleteMessage: Remove processed message.

ChangeMessageVisibility: Extend processing time.

Batch APIs: Send, Delete, ChangeVisibility in bulk → fewer calls, lower cost.

0:00
19. AWS Integration & Messaging: SQS, SNS & Kinesis
226. SQS - FIFO Queues Advanced
SQS FIFO (First-In-First-Out)

Keeps message order (per Message Group ID)

Name must end with .fifo

Throughput: 300 msg/s (no batch), 3000 msg/s (batch)

Dedup interval: 5 min

Dedup methods:

Content-based (SHA-256 of body)

Explicit ID (MessageDeduplicationId)

Message Group ID (mandatory):

Same ID → ordered, single consumer

Different IDs → parallel consumers, order per group

Use cases: transaction order, chat threads, per-customer order processing

0:00
19. AWS Integration & Messaging: SQS, SNS & Kinesis
227. Amazon SNS
Amazon SNS (Simple Notification Service)

Pub/Sub model: Producer → SNS Topic → multiple subscribers

Subscribers: Email, SMS, HTTP(S) endpoints, Lambda, SQS, Kinesis Firehose

Limits: ~12M+ subs/topic, 100K topics/account (limits can increase)

Integration: Many AWS services publish to SNS (e.g., CloudWatch, S3, ASG, DynamoDB, RDS)

Security:

In-flight encryption (TLS), At-rest (KMS), optional client-side encryption

IAM policies + SNS access policies (cross-account or service access)

Mobile push: Direct publish via platform endpoints

Google Cloud Messaging (GCM)

Apple Push Notification Service (APNS)

Amazon Device Messaging (ADM)

0:02
19. AWS Integration & Messaging: SQS, SNS & Kinesis
228. Amazon SNS and SQS - Fan Out Pattern
SNS + SQS Fan-out Pattern
Producer sends 1 message to an SNS topic.

Multiple SQS queues subscribe to the topic and each get the message.

Benefits: decoupled, reliable, scalable, no data loss, supports retries & delayed processing.

Add/remove SQS subscribers anytime without changing producers.

SQS queues must allow SNS in their access policy.

Cross-region delivery possible if allowed by security.

Works with FIFO SNS topics and FIFO SQS queues for ordered, deduplicated messages.

Message filtering: subscribers can filter messages by JSON policies (e.g., only State=Placed).

Without filters, subscribers get all messages.

1:50
19. AWS Integration & Messaging: SQS, SNS & Kinesis
230. Amazon Kinesis Data Streams
0:00
19. AWS Integration & Messaging: SQS, SNS & Kinesis
233. Amazon Data Firehose - Hands On
Firehose
Purpose: Fully managed service to load streaming data into destinations.

Sources:

Direct PUT via SDK, Kinesis Agent

AWS services (CloudWatch Logs/Events, IoT Core, EventBridge)

Kinesis Data Streams (pull)

Destinations:

AWS: S3, Redshift, OpenSearch

Third-party: Splunk, Datadog, New Relic, MongoDB

Custom HTTP endpoint

Features:

Optional Lambda transform (format conversion, filtering, etc.)

Convert to Parquet/ORC, compress (GZIP, Snappy, ZIP)

Store failed/all data to S3 for backup

Automatic scaling, fully serverless, pay-per-use

Near real-time (buffer by size/time → flush)

Supported formats: CSV, JSON, Parquet, Avro, text, binary

No storage/replay capability (unlike Kinesis Data Streams)

1:50
19. AWS Integration & Messaging: SQS, SNS & Kinesis
233. Amazon Data Firehose - Hands On
Key Configurations
Buffer Size: Default 5 MB (up to 128 MB) – bigger = efficiency, smaller = faster delivery.

Buffer Interval: Default 300s; min 60s, max 900s.

Compression: Saves cost (GZIP, Snappy, etc.).

Encryption: Optional, configure at destination level.

IAM Role: Auto-created with permissions to read source & write destination.

2:35
19. AWS Integration & Messaging: SQS, SNS & Kinesis
233. Amazon Data Firehose - Hands On
Firehose vs Kinesis Data Streams (Key Differences)

Delivery speed: Firehose is near real-time (due to buffering). Kinesis Data Streams is real-time.

Scaling: Firehose scales automatically. Kinesis Data Streams can be provisioned or on-demand.

Storage: Firehose has no storage. Kinesis Data Streams can store data for up to 1 year.

Replay: Firehose has no replay capability. Kinesis Data Streams supports replay.

Coding: Firehose requires no custom consumer code. Kinesis Data Streams requires you to write consumer/producer code.

Destinations: Firehose sends directly to AWS services (S3, Redshift, OpenSearch), supported third parties, or HTTP endpoints. Kinesis Data Streams delivers to custom consumers.

3:46
19. AWS Integration & Messaging: SQS, SNS & Kinesis
233. Amazon Data Firehose - Hands On
Exam Tips
“Near real-time” → Think Data Firehose (due to buffering).

For real-time with replay → Kinesis Data Streams.

Remember main destinations: S3, Redshift, OpenSearch.

Use Lambda for custom transformations.

Failed data can be backed up to S3.

Serverless, pay for usage, no manual scaling.

0:00
20. AWS Monitoring & Audit: CloudWatch, X-Ray and CloudTrail
241. CloudWatch Logs - Hands On
CloudWatch Metrics
Default metrics: provided automatically by AWS services.

Custom metrics: use PutMetricData API.

Dimensions: key/value to refine (e.g., InstanceId, Env).

Storage resolution:

Standard = 60s

High resolution = 1s, 5s, 10s, 30s

Timestamps:

Allowed 2 weeks past or 2 hours future

⚠️ Must have correct instance clock time.

CloudWatch Agent: pushes system metrics (RAM, disk, etc.) via PutMetricData.

Namespace: logical container for metrics (e.g., AWS/EC2, or custom MyApp).

0:01
20. AWS Monitoring & Audit: CloudWatch, X-Ray and CloudTrail
245. CloudWatch Logs - Metric Filters Hands On
CloudWatch Logs
• Log Groups – app/service level
• Log Streams – instance/container/file level
• Retention – 1 day–10 yrs or never
• Encryption – default or KMS CMK

Sources
• EC2 / On-prem – CloudWatch Agent (Unified = logs + metrics)
• ECS / Lambda / Beanstalk / API GW / VPC Flow / CloudTrail / Route53 – auto logs

Agents
• Logs Agent – old, logs only
• Unified Agent – logs + metrics (CPU, RAM, Disk IO, Netstats, Processes, Swap)

Querying
• Logs Insights – query/filter/aggregate/save dashboards
• Live Tail – real-time, 1 hr/day free

Export & Streaming
• S3 Export – batch, ~12 hrs
• Subscription Filters – real-time → Kinesis / Firehose / Lambda / OpenSearch, max 2 per log group, cross-account

Metric Filters → Alarms
• Metric Filter – extract metrics, not retroactive
• Alarm – trigger SNS / Lambda (e.g., ≥5 errors/min)

0:00
20. AWS Monitoring & Audit: CloudWatch, X-Ray and CloudTrail
247. CloudWatch Alarms Hands On
CloudWatch Alarms



• Purpose – trigger actions/notifications from metrics



• States – OK, INSUFFICIENT_DATA, ALARM



• Period – evaluation interval; supports high-res metrics (10s, 30s, 1min+)



• Actions/Targets – EC2: stop, terminate, reboot, recover; Auto Scaling: scale in/out; SNS: notifications, trigger Lambda



• Composite Alarms – combine multiple alarms with AND/OR, reduce noise, flexible conditions



• EC2 Recovery – instance, system, EBS checks; retains IPs, metadata, placement; can alert via SNS



• Integration/Testing – metric filters → alarms → SNS; test alarms via CLI/API set-alarm-state



• Exam Focus – single/multiple metrics, automated EC2/Auto Scaling actions, high-res metrics, works with CloudWatch Logs metric filters

2:53
20. AWS Monitoring & Audit: CloudWatch, X-Ray and CloudTrail
248. CloudWatch Synthetics
CloudWatch Synthetics Canary

AWS service feature used to monitor your applications and APIs by simulating user behavior.

• Runs on schedule (e.g., every 1–5 min)

• Scripts in Node.js or Python

• Logs failures in CloudWatch, trigger SNS alarms

• Use cases – API/website health checks, uptime, response validation

1:16
20. AWS Monitoring & Audit: CloudWatch, X-Ray and CloudTrail
263. CloudTrail vs CloudWatch vs X-Ray
CloudTrail vs CloudWatch vs X-Ray
CloudTrail → Auditing

Records API calls (by users, services, console).

Detect unauthorized activity, investigate root cause of changes.

CloudWatch → Monitoring

Metrics: monitor performance.

Logs: store application/system logs.

Alarms: trigger actions/notifications on unexpected metrics.

X-Ray → Debugging & Tracing

Distributed request tracing, service map.

Helps analyze latency, errors, faults.

Request tracking across microservices.

👉 CloudTrail = Who did what (API)
👉 CloudWatch = How system is performing (metrics/logs)
👉 X-Ray = Where request spent time (tracing/debugging)

0:03
20. AWS Monitoring & Audit: CloudWatch, X-Ray and CloudTrail
263. CloudTrail vs CloudWatch vs X-Ray
example:
CloudWatch Metrics & Alarms

Detects CPU > 80% on EC2.

Alarm gets triggered.

Notification / Automation

SNS: Sends email/SMS/Slack → “CPU High on EC2”.

EventBridge: Triggers automated actions → start a Lambda, scale ASG, create a Jira ticket.

Debugging the Application

X-Ray: Shows root cause at the application/request level.

Example: /getUser API is calling DynamoDB, and that query is super slow.

Auditing & Security

CloudTrail: Records who/what made changes that could explain the issue.

Example: Someone modified the DynamoDB read capacity, or deployed a new Lambda, or updated EC2 security groups.

Lets you answer “was this caused by code changes or a misconfiguration?”

0:17
20. AWS Monitoring & Audit: CloudWatch, X-Ray and CloudTrail
263. CloudTrail vs CloudWatch vs X-Ray
How they work together

CloudWatch → detects the problem

SNS → notifies people

EventBridge → triggers automated actions

X-Ray → shows the root cause in the app

CloudTrail → shows who/what caused the change

CloudTrail Management Events → Audit trail (who/what changed it).

CloudTrail Insights → Detect unusual activity.

2:06
21. AWS Serverless: Lambda
266. Serverless Introduction
AWS Serverless :
Compute
AWS Lambda – Run code without managing servers.

AWS Fargate – Run Docker containers serverlessly.

AWS Step Functions – Orchestrate workflows without managing servers.

Databases
Amazon DynamoDB – Serverless NoSQL database.

Aurora Serverless – Relational database that scales automatically.

Storage
Amazon S3 – Object storage, fully managed and serverless.

Messaging & Streaming
Amazon SNS – Pub/Sub notifications, auto-scaled.

Amazon SQS – Managed message queues, auto-scaled.

Amazon Kinesis Data Firehose – Real-time streaming, auto-scaled, pay-per-use.

Identity & Access
Amazon Cognito – Serverless user authentication & authorization.

1:31
21. AWS Serverless: Lambda
276. Lambda & CloudWatch Events / EventBridge Hands On
event bridge + lambda ---> used to create kind of a cron job. - serverless

0:03
21. AWS Serverless: Lambda
279. Lambda Event Source Mapping
Event source mapping lets Lambda poll streams/queues (SQS, Kinesis, DynamoDB Streams) and invoke functions with batches of records. Push-based services (S3, API Gateway, EventBridge, SNS) trigger Lambda directly, so no mapping needed.

0:42
21. AWS Serverless: Lambda
279. Lambda Event Source Mapping
👉 Trick for exam:

Streams + Queues = Needs event source mapping

Events/Notifications = Direct trigger.  So, event source mapping NOT needed

1:13
21. AWS Serverless: Lambda
282. Lambda Destinations
Lambda Destinations route the result of asynchronous Lambda executions.

On Success / On Failure → send to SNS, SQS, EventBridge, or another Lambda.

More flexible than DLQ (which only handles failures and only with SQS/SNS).

0:06
21. AWS Serverless: Lambda
284. Lambda Permissions - IAM Roles & Resource Policies
Execution role = Lambda’s permissions to AWS services

Resource-based policy = Who can invoke the Lambda function

1:29
21. AWS Serverless: Lambda
288. Lambda Monitoring & X-Ray Tracing
Lambda automatically sends logs to CloudWatch Logs, and you can also use X-Ray for tracing function execution and performance.

4:13
21. AWS Serverless: Lambda
290. Lambda@Edge & CloudFront Functions
🔹 Lambda@Edge
Extension of Lambda, but runs in CloudFront Edge locations worldwide.

AWS automatically replicates function to edges.

Use cases: auth at edge, dynamic content routing, URL rewrites, header manipulation.

Supports viewer/origin request/response events in CloudFront.

Limited runtimes vs normal Lambda (e.g., only Node.js, Python).

Higher latency than CloudFront Functions (since it spins up container runtime).

4:18
21. AWS Serverless: Lambda
290. Lambda@Edge & CloudFront Functions
🔹 CloudFront Functions
Lightweight JS functions running directly at CloudFront Edge locations.

Millisecond execution, very cheap.

Only supports JavaScript.

Best for: very high-volume, simple logic

URL rewrites/redirects

Header manipulation

Basic authentication checks

Cannot access other AWS services directly (unlike Lambda).

4:23
21. AWS Serverless: Lambda
290. Lambda@Edge & CloudFront Functions
👉 Exam Tip:

If the question says “very high volume, lightweight header/URL logic” → CloudFront Functions.

If “need personalization, access AWS services, or heavier edge compute” → Lambda@Edge.

If “backend or regional service” → plain Lambda.

0:10
21. AWS Serverless: Lambda
293. Lambda Function Performance
By default, Lambda is not in your VPC — but it can still reach the internet and AWS services.

If you attach Lambda to a VPC + private subnets, it loses internet access unless you add a NAT Gateway.

VPC attachment is only needed if Lambda must access private resources (RDS, EC2, ElastiCache).

👉 Remember: “Lambda in VPC = no internet by default. Add NAT if you need it.”

1:17
21. AWS Serverless: Lambda
293. Lambda Function Performance
If your lambda function is CPU-bound (computation heavy), increase RAM.

Timeout - default 3 secs maximum is 900 sec (15 mins)

2:29
21. AWS Serverless: Lambda
293. Lambda Function Performance
The Lambda execution context is the temporary runtime AWS reuses for function calls (memory, /tmp storage, initialized variables). Warm starts reuse this context and are faster, while cold starts set up a new one.

👉 Example: If you open a DB connection outside the handler, it can be reused across warm starts—saving time and cost.

1:51
21. AWS Serverless: Lambda
295. Lambda Layers
Lambda Layers & Custom Runtimes

Layers = share code/dependencies across functions (e.g., libraries, configs).

Max 5 layers per function → keeps packages small & consistent.

Custom runtimes = your own language/runtime (e.g., Ruby, older Python).

They’re delivered via a layer with a bootstrap file that defines how Lambda runs your code.

👉 Example: Put numpy in a layer for reuse OR create a runtime layer with bootstrap to run a non-AWS language.

0:00
21. AWS Serverless: Lambda
297. Lambda File Systems Mounting
Lambda storage options ✅

Ephemeral storage (/tmp):

512 MB by default, configurable up to 10 GB.

Lives only for the function’s execution context (lost after container is destroyed).

Good for temp files, caching during warm starts.

Persistent storage (external):

S3 → store/retrieve large files.

EFS (Elastic File System) → attach shared file system to Lambda (up to PB scale).

Used for data that must persist beyond one invocation.

👉 Rule of thumb: /tmp for scratch space, S3/EFS for lasting storage.

1:11
21. AWS Serverless: Lambda
298. Lambda Concurrency
Reserved concurrency = guarantees a set number of concurrent executions for a Lambda and blocks extra invocations beyond that limit. Reserved concurrency means “Lambda can run up to this many instances at the same time; any extra requests wait or fail.

0:10
21. AWS Serverless: Lambda
302. Lambda and CloudFormation
You package dependencies with your Lambda code by zipping them together, or by using Lambda Layers to keep dependencies separate and reusable. For Python/Node.js, install the packages into your project folder (e.g., pip install -t .), zip everything, and upload.

1:59
21. AWS Serverless: Lambda
304. Lambda Container Images
Lambda supports container images (new feature).

Size: up to 10 GB from ECR.

Use AWS base images (Node.js, Python, Java, .NET, Go, Ruby) → already implement Lambda Runtime API.

Or build your own base image (must implement Runtime API).

Workflow: Build Docker image → push to ECR → deploy to Lambda.

Can test locally with Runtime Interface Emulator.

Best practices:

Use AWS base images (cached, faster).

Use multi-stage builds → smaller image.

Order layers: stable stuff first, frequently changing last.

Keep large functions in single ECR repo to avoid duplicate layers.

Use case: Functions with large dependencies or up to 10 GB code/data.

👉 Memory hook: “Lambda as a container = ECR 10GB, base image + code + deps, must support Runtime API.”

5:47
21. AWS Serverless: Lambda
306. Lambda Versions and Aliases - Hands On
In Lambda, versions are immutable snapshots of your function code and settings, while $LATEST is the always-changing version. Aliases act like stable pointers to specific versions (e.g., PROD → v5, DEV → v7) and can also split traffic between versions for gradual rollouts.

Use case: You deploy v6 of a Lambda, point the BETA alias to it, and send 10% of traffic there while 90% still goes to v5 (PROD). If v6 works fine, update PROD alias to point fully to v6.

👉 Think: Versions = snapshots, Aliases = friendly names + traffic shifting.

0:01
21. AWS Serverless: Lambda
311. Lambda Limits
Execution limits (per region):

Memory: 128 MB – 10 GB (64 MB increments). More memory = more vCPU.

Timeout: Max 900s (15 min).

Env variables: Max 4 KB.

/tmp storage: Up to 10 GB.

Concurrency: Default 1,000 concurrent executions (can request increase).

Deployment limits:

Code package size:

50 MB compressed (zip).

250 MB uncompressed.

For larger dependencies → use Lambda Layers or container images (up to 10 GB).

👉 Memory hook: 15 min, 10 GB RAM, 10 GB /tmp, 4 KB env vars, 1k conc, 50/250 MB code size.

0:50
21. AWS Serverless: Lambda
312. Lambda Best Practices
Lambda Best Practices:

Heavy work outside handler: Initialize DB connections, AWS SDK, dependencies outside the handler to reduce execution time.

Use environment variables: For configs like DB strings, S3 bucket names; encrypt sensitive values with KMS.

Minimize deployment package: Include only runtime necessities; break large functions; use Lambda Layers for shared libraries.

Avoid recursion: Do not let a Lambda call itself → expensive and risky.

👉 Memory hook: Handler = fast; configs/env = variables/KMS; package = small; reuse = layers; no self-calls.

0:00
21. AWS Serverless: Lambda
308. Lambda Function URL
Lambda Function URL: Built-in HTTPS endpoint to invoke a Lambda function directly (no API Gateway needed).

Use case: Quickly expose a Lambda as a web endpoint for HTTP requests.

Auth types:

NONE → public access (anyone can call).

AWS_IAM → secure access using IAM credentials.

CORS: Can enable to allow browser apps to call from another domain.

Limitations: Same Lambda limits apply (memory, timeout, payload size, concurrency).

Quick setup:

Go to Lambda console → Functions → Function URL → Create URL.

Choose auth type.

Optionally configure CORS.

👉 Memory hook: Function URL = simple HTTPS endpoint for Lambda, can be public or IAM-protected.

1:33
21. AWS Serverless: Lambda
308. Lambda Function URL
Lambda Function URL vs API Gateway:

Setup: Function URL → super quick, built-in HTTPS endpoint. API Gateway → more configuration required.

Auth: Function URL → NONE (public) or AWS_IAM. API Gateway → multiple options (IAM, Cognito, custom authorizers).

Features: Function URL → basic HTTP invoke, optional CORS. API Gateway → request/response transformation, caching, throttling, stages.

Use case: Function URL → simple, quick endpoint for a single Lambda. API Gateway → complex APIs with routing, versioning, and monitoring.

Cost & overhead: Function URL → lightweight, minimal overhead. API Gateway → more features but higher cost and latency.

👉 Memory hook: Function URL = fast & simple, API Gateway = full-featured API solution.

0:02
21. AWS Serverless: Lambda
310. Lambda - CodeGuru Integration
AWS Lambda integrates with Amazon CodeGuru Profiler to help you monitor and optimize the performance of your serverless applications

0:05
21. AWS Serverless: Lambda
307. Lambda and CodeDeploy
common deployment methods in AWS / general DevOps context:

All-at-once / Big Bang: Deploy new version to all users at once – fast but risky.

Canary: Roll out to a small subset first, monitor, then expand – reduces risk.

Linear / Rolling: Gradually shift traffic in equal increments over time until 100% – predictable and safe.

Blue/Green: Run new version in parallel (green), switch traffic from old version (blue) instantly – zero downtime, easy rollback.

Immutable: Deploy new version to new servers/containers without touching old ones – rollback = easy, avoids config drift.

A/B Testing: Route traffic to different versions to test features or performance – mainly for experiments and feature validation.

3:46
22. AWS Serverless: DynamoDB
321. DynamoDB Indexes (GSI + LSI)
If the writes are throttled n the GSI, then the main table will be throttled.

0:02
22. AWS Serverless: DynamoDB
313. DynamoDB - Section Introduction
Basics
Fully managed NoSQL database (key-value and document store).

Serverless, scales automatically (On-Demand) or via provisioned capacity.

Highly available & durable (multi-AZ).

Key Concepts:

Table: Collection of items.

Item: Row (document).

Attributes: Columns (key-value pairs).

Primary Key: Partition Key (hash) + optional Sort Key (range).

Secondary Indexes:

Global Secondary Index (GSI): Any attribute as PK/SK, cross-partition.

Local Secondary Index (LSI): Same partition key as table, different sort key.

0:07
22. AWS Serverless: DynamoDB
313. DynamoDB - Section Introduction
Capacity & Throughput
Provisioned Capacity
WCU (Write Capacity Unit): 1 WCU = 1 write/sec for 1 KB.

RCU (Read Capacity Unit):

Strongly consistent read → 1 RCU = 1 read/sec per 4 KB item.

Eventually consistent read → 1 RCU = 2 reads/sec per 4 KB item.

Auto Scaling: Adjust WCU/RCU automatically.

On-Demand Capacity
Pay per request. Automatic scaling, no WCU/RCU provisioning.

Transactional Capacity
Transactional operations cost 2x capacity.

TransactWriteItems: 2x WCU

TransactGetItems: 2x RCU

Example:

3 transactional writes/sec, 5 KB items → 3*5*2 = 30 WCU.

5 transactional reads/sec, 5 KB items → round 5 KB → 8 KB → 5*8/4*2 = 20 RCU.

0:09
22. AWS Serverless: DynamoDB
313. DynamoDB - Section Introduction
DynamoDB Streams
Captures item-level changes (Insert, Modify, Remove).

Used for triggers, replication, analytics.

Options:

KEYS_ONLY

NEW_IMAGE

OLD_IMAGE

NEW_AND_OLD_IMAGES

Integration with Lambda:

Lambda function triggered per batch of changes.

Logs records in CloudWatch.

Requires Lambda execution role with DynamoDB read access.

Use Case:

Event-driven processing, audit logs, replication.

0:10
22. AWS Serverless: DynamoDB
313. DynamoDB - Section Introduction
Time To Live (TTL)
Auto-delete items after expiration timestamp (Epoch number).

No WCU consumed for TTL deletes.

Expired items deleted asynchronously (up to ~40 hours).

Deletes appear in DynamoDB Streams.

Use Cases: Session data, temporary records, regulatory retention limits.

5️⃣ DynamoDB Transactions
ACID: Atomic, Consistent, Isolated, Durable.

Read Modes: Eventual, Strong, Transactional consistency.

Write Modes: Standard, Transactional (all-or-nothing).

API Calls:

TransactGetItems → Read multiple items transactionally.

TransactWriteItems → Put/Update/Delete multiple items transactionally.

Use Cases:

Banking, orders, multiplayer games.

0:12
22. AWS Serverless: DynamoDB
313. DynamoDB - Section Introduction
DynamoDB Writes
Types of Writes
Concurrent Writes: Multiple writes at the same time; last write wins.

Conditional Writes (Optimistic Locking): Write only if condition matches.

Atomic Counters: Increment/decrement values atomically.

Batch Writes: Write multiple items in a single operation.

7️⃣ DynamoDB CLI Tips
Projection Expression: Retrieve subset of attributes.

Filter Expression: Client-side filtering of results.

Pagination Options:

--page-size: API call size, helps avoid timeouts.

--max-items: Limit items per command.

--starting-token: Continue from previous results (NextToken).

0:14
22. AWS Serverless: DynamoDB
313. DynamoDB - Section Introduction
DynamoDB & Large Objects / S3
1. Store large objects externally:

Store large items (images, videos) in S3.

Store metadata (product ID, image URL) in DynamoDB.

Read: Query DynamoDB → fetch object from S3.

2. Index S3 metadata:

Upload S3 object → trigger Lambda → write metadata to DynamoDB.

Query metadata efficiently (timestamps, owner, attributes).

0:15
22. AWS Serverless: DynamoDB
313. DynamoDB - Section Introduction
DynamoDB Sharding
Hot partitions problem: e.g., votes on candidate A/B → all writes go to same partition.

Solution: Add random prefix or suffix to partition key → distributes load.

Methods: Random number or hash.

0:22
22. AWS Serverless: DynamoDB
313. DynamoDB - Section Introduction
Security
IAM: Full access control.

VPC Endpoints: Access DynamoDB privately without public internet.

Encryption:

At rest → KMS

In transit → SSL/TLS

Backup & Restore:

Point-in-time Recovery (PITR)

Manual backup/restore

Global Tables: Multi-region, multi-master replication.

Requires DynamoDB Streams.

Fine-grained access:

Use federated login (Cognito, OIDC, SAML).

Map temporary credentials → IAM role → condition on LeadingKeys (row-level access).

Can also limit attribute-level access.

0:23
22. AWS Serverless: DynamoDB
313. DynamoDB - Section Introduction
Exam Tips
Always know WCU/RCU calculations.

Streams + Lambda integration is key for event-driven designs.

TTL deletes → no WCU, shows up in streams.

Transactions = ACID, 2x capacity.

Sharding = avoid hot partitions.

Use DynamoDB for serverless session storage, S3 for large objects, ElastiCache for in-memory caching.

Fine-grained access → IAM + conditions (LeadingKeys / attributes).

0:19
22. AWS Serverless: DynamoDB
313. DynamoDB - Section Introduction
Session State / Caching
DynamoDB

Description: Serverless key-value store

When to Use: Auto-scaling sessions, durable, cross-region

ElastiCache

Description: In-memory (Redis/Memcached)

When to Use: Low-latency session store

EFS

Description: Shared file system

When to Use: File-based session state

EBS / Instance Store

Description: Local storage

When to Use: Only local cache (single instance)

S3

Description: Object storage

When to Use: Not ideal for session state (high latency)

💡 Tip: Exam focuses on in-memory (ElastiCache) vs serverless (DynamoDB).

0:33
22. AWS Serverless: DynamoDB
313. DynamoDB - Section Introduction
Capacity
Provisioned
 ├─ RCU (Read Capacity Unit)
 │   ├─ Strong → 1 RCU = 1 read/sec per 4 KB
 │   └─ Eventually → 1 RCU = 2 reads/sec per 4 KB
 └─ WCU (Write Capacity Unit)
     └─ 1 WCU = 1 write/sec per 1 KB
     
Transactional → 2x capacity
0:35
22. AWS Serverless: DynamoDB
313. DynamoDB - Section Introduction
--projection-expression → retrieve specific attributes

--filter-expression     → client-side filtering

--page-size             → API call optimization

--max-items             → limit items per call

--starting-token        → fetch next page (NextToken)

1:44
23. AWS Serverless: API Gateway
358. API Gateway - Architecture
Amazon API Gateway – DVA Exam Refresher
1. What is API Gateway?
Fully managed service for creating, publishing, maintaining, monitoring, and securing APIs at scale (REST, HTTP, and WebSocket) AWS Documentation+1.

Acts as the "front door" for applications to access backend services like Lambda, EC2, Step Functions, and third-party APIs AWS Documentation.

2. API Types
REST APIs

Feature-rich: usage plans, API keys, request/response transformations MediumAWS Documentation.

HTTP APIs

Lightweight, lower latency and cost; ideal for basic APIs Medium.

WebSocket APIs

Support real-time, stateful, full-duplex communication Medium.

0:01
23. AWS Serverless: API Gateway
358. API Gateway - Architecture
3. Core Concepts
Resources & Methods: Build a resource tree with methods (GET, POST, etc.) mapped to backend integrations AWS Documentation.

Stages: Represent deployment lifecycle like dev, test, prod AWS StaticAWS Documentation.

Endpoints Types:

Edge-optimized: Uses CloudFront for global access.

Regional: Targets specific AWS region.

Private: Accessible only via VPC endpoints (no public internet access) Tutorials DojoAWS Documentation.

4. Security Options
Supports IAM policies, Lambda authorizers, Cognito user pools, and AWS WAF integration for securing APIs AWS DocumentationTutorials Dojo.

Enforce request signing (SigV4) with IAM for authenticated access Tutorials Dojo.

0:07
23. AWS Serverless: API Gateway
358. API Gateway - Architecture
5. Performance & Traffic Management
Throttling: Set per-method rate limits and burst controls.

Caching: API-level cache with TTL to reduce backend load Tutorials Dojo.

Canary Deployments: Safely roll out API changes to a subset of users AWS Documentation.

6. Monitoring and Logging
Integrates with CloudWatch for API metrics, execution logs, and alarms.

Integrates with CloudTrail for audit tracking.

Can integrate with AWS X-Ray for tracing and performance debugging AWS Documentation.

7. CI/CD and Infrastructure as Code
Supports CloudFormation, AWS SAM, and CDK deployments AWS DocumentationAWS Static.

Key capabilities:

Manage multiple stages.

Use custom domains.

Integrate with deployment strategies like canary/blue-green

0:37
23. AWS Serverless: API Gateway
358. API Gateway - Architecture
8. Exam Relevance
A primary service in the Serverless architecture domain (with Lambda & DynamoDB) MediumReddit.

Expect questions on:

Differentiating REST vs HTTP vs WebSocket APIs.

Secure access patterns using IAM/Cognito/Lambda authorizers/WAF.

Handling CORS configuration (common scenario) Tutorials Dojo.

Integration strategies with backend services.

Deployment via stages, usage plans, and CI/CD tooling.



0:40
23. AWS Serverless: API Gateway
358. API Gateway - Architecture
Quick Summary for Exam Review
Know the three API types and use cases.

Understand deployment stages, endpoint types, and security options.

Be ready to configure caching, throttling, and monitoring.

Understand integration with Lambda, third-party endpoints, and authorization mechanisms.

Be familiar with deployment automation (i.e., IaC, stages, canary releases).

1:38
24. AWS CICD: CodeCommit, CodePipeline, CodeBuild, CodeDeploy
376. CodeGuru - Agent Configuration
Blue/Green deployment → CodeDeploy.

CI/CD pipeline automation → CodePipeline.

Build/test automation → CodeBuild.

Git repo in AWS → CodeCommit.

Dependency repo → CodeArtifact.

ML-based code insights → CodeGuru.

Quick browser CLI → CloudShell.

4:26
25. AWS Serverless: SAM - Serverless Application Model
378. SAM Overview
🚀 SAM – AWS Certified Developer Associate Cheat Sheet
📌 What is SAM?
Framework on top of CloudFormation → simplifies serverless app deployment.

Focused on Lambda, API Gateway, DynamoDB, Step Functions, S3, EventBridge.

🛠 Key Concepts
template.yaml → defines serverless resources.

Resources shorthand:

AWS::Serverless::Function → Lambda

AWS::Serverless::Api → API Gateway

AWS::Serverless::Table → DynamoDB

Uses CloudFormation under the hood.

0:02
25. AWS Serverless: SAM - Serverless Application Model
378. SAM Overview
⚡ Common SAM CLI Commands
sam build → build dependencies.

sam local invoke → run Lambda locally.

sam local start-api → run API Gateway + Lambda locally.

sam package → package & upload to S3.

sam deploy --guided → deploy stack via CloudFormation.

0:30
25. AWS Serverless: SAM - Serverless Application Model
378. SAM Overview
🔄 Deployment + CI/CD
Works with CodeDeploy for Lambda traffic shifting:

Canary (10% for X mins, then 100%).

Linear (10% every N mins).

All-at-once.

Can integrate with CodePipeline.

🎯 Exam Must-Know

SAM = CloudFormation extension for serverless.

Lets you test locally before deploying.

Simplifies Lambda + API Gateway deployments.

Supports safe Lambda deployments (CodeDeploy + traffic shifting).

You don’t need to memorize syntax — just purpose + commands + integrations.

1:16
25. AWS Serverless: SAM - Serverless Application Model
378. SAM Overview
✅ If the exam asks:

“How to test Lambda + API Gateway locally?” → SAM CLI (sam local).

“Which tool helps define + deploy serverless apps with less boilerplate?” → SAM.

“How to shift traffic safely between Lambda versions?” → SAM + CodeDeploy.

2:07
25. AWS Serverless: SAM - Serverless Application Model
381. SAM Policy Templates
📝 SAM Policy Templates – DVA Refresher
What are they?
Prebuilt IAM policy snippets you can attach to a Lambda in your SAM template.yml.

Save time: you don’t need to write JSON policies manually.

SAM expands them into full IAM policies under the hood.

Common Templates to Know (Exam-Friendly)
S3ReadPolicy → Lambda can read objects from S3 bucket(s).

SQSPollerPolicy → Lambda can poll messages from SQS.

DynamoDBCrudPolicy → Lambda can create, read, update, delete items in DynamoDB table.

SNSPublishPolicy → Lambda can publish messages to SNS topic(s).

VPCAccessPolicy → Lambda can access resources in a VPC.

1:37
25. AWS Serverless: SAM - Serverless Application Model
381. SAM Policy Templates
example:



Resources:

  MyFunction:

    Type: AWS::Serverless::Function

    Properties:

      CodeUri: src/

      Handler: app.handler

      Runtime: python3.9

      Policies:

        - S3ReadPolicy:

            BucketName: my-bucket



0:01
26. Cloud Development Kit (CDK)
389. CDK - Unit Testing
AWS CDK (Cloud Development Kit) – Simple Terms
What it is:
A tool to define AWS infrastructure using real programming languages (like Python, JavaScript, Java, TypeScript, C#).

Why it exists:
Instead of writing long YAML/JSON templates in CloudFormation, you can write code in a language you already know.

How it works (big picture):

You write infrastructure as code in your chosen language (say Python).

CDK converts your code into CloudFormation templates under the hood.

CloudFormation then creates the AWS resources.

Example (broad idea):
Instead of writing YAML for an S3 bucket, in CDK you could just write something like: bucket = s3.Bucket(self, "MyBucket")

0:10
26. Cloud Development Kit (CDK)
389. CDK - Unit Testing
Benefits:

Use loops, conditions, functions (normal programming) to create infrastructure.

Easier to maintain when you have many resources.

Works well for developers who prefer code over YAML/JSON.

Downside:

Adds an extra layer to learn.

Still depends on CloudFormation in the end.

0:00
27. Cognito: Cognito User Pools, Cognito Identity Pools & Cognito Sync
390. Cognito Overview
📝 Cognito – DVA Exam Notes
1️⃣ What is Cognito?
Identity & Access Management for Apps (not AWS console users → that’s IAM).

Helps users sign up, sign in, and access resources securely.

Works for web and mobile apps.

0:00
27. Cognito: Cognito User Pools, Cognito Identity Pools & Cognito Sync
391. Cognito User Pools
2️⃣ Two Main Components
1. Cognito User Pools 🏊
User directory for app authentication (who you are).

Handles sign-up, sign-in, MFA, password reset.

Issues JWT tokens (ID, Access, Refresh).

Supports social logins (Google, Facebook, Apple) and SAML/enterprise IdPs.

Example exam point: “App users authenticate via Cognito → app gets tokens → uses tokens to call API Gateway.”

2. Cognito Identity Pools 🪪
Provide temporary AWS credentials via STS.

Used for authorization (what you can access).

Can assign different IAM roles based on user identity.

Example:

Authenticated users → role with full S3 access.

Guest (unauthenticated) users → role with read-only S3 access.

0:00
27. Cognito: Cognito User Pools, Cognito Identity Pools & Cognito Sync
397. Cognito User Pools vs Cognito Identity Pools
3️⃣ Key Features
Federated Identity: unify login across social IdPs + corporate IdPs + user pools.

Fine-grained access control: Map users/groups to IAM roles.

Secure token exchange: Cognito → AWS STS → IAM roles.

Scales automatically for millions of users.

4️⃣ Exam Traps 🚨
IAM vs Cognito:

Use IAM for employees/internal AWS users.

Use Cognito for external app users (customers).

User Pool vs Identity Pool:

User Pool = Authentication.

Identity Pool = Authorization + AWS credentials.

Cognito issues JWT tokens, not IAM access keys directly.

Cognito often paired with API Gateway (JWT token validation).



1:30
27. Cognito: Cognito User Pools, Cognito Identity Pools & Cognito Sync
397. Cognito User Pools vs Cognito Identity Pools
5️⃣ Typical Exam Scenarios
Scenario 1: Mobile app wants to allow users to sign in with Google → Use User Pool with Google IdP.

Scenario 2: App users need to upload to S3 after sign-in → Use Identity Pool to grant temporary AWS creds.

Scenario 3: Distinguish between authenticated vs unauthenticated users for S3 permissions.

Scenario 4: Validate JWT token from Cognito before allowing access to API Gateway.

✅ Summary in one line for memory:

Cognito User Pool = Auth (login, JWT).

Cognito Identity Pool = AuthZ (temporary AWS creds via STS).

0:01
29. Advanced Identity
410. STS Overview
📝 AWS STS (Security Token Service) – DVA Exam Notes
What it is:

Service to create temporary security credentials (short-lived).

Useful for cross-account access or federated identities.

Key Points for Exam:

AssumeRole: Most important API → lets you switch roles (cross-account or within account).

Federation: Works with external identity providers (SAML, Cognito, corporate AD).

Short-lived creds = safer (reduce long-term exposure).

Common in CI/CD pipelines, cross-account Lambda execution, federated logins.

Exam Traps 🚨:

If question mentions temporary credentials, cross-account, or federated users → think STS.

Don’t confuse with Cognito (Cognito is for app users, STS is for AWS roles/accounts).

0:00
29. Advanced Identity
413. AWS Directory Services
📝 AWS Directory Service – DVA Exam Notes
What it is:

AWS’s managed Microsoft Active Directory (AD) in the cloud.

Lets AWS resources work with Windows-based identity & group policies.

Flavors:

AWS Managed Microsoft AD → Full, managed AD in AWS.

AD Connector → Proxy to your existing on-prem AD (no sync).

Simple AD → Lightweight, low-cost AD-compatible service.

Key Integrations:

EC2 Windows instances → Join domain.

Workspaces, QuickSight, RDS SQL Server → Integrate with AD for authentication.

Exam Traps 🚨:

If scenario mentions Windows workloads needing domain join → AWS Directory Service.

If scenario mentions centralized user management for Windows apps → Directory Service.

Not usually mixed with Lambda/serverless questions (more infra + Windows focus).



2:02
29. Advanced Identity
413. AWS Directory Services
✅ Quick Memory Hook:

STS = Temporary credentials, roles, federation.

Directory Service = Active Directory in AWS, Windows domain join.

3:53
30. AWS Security & Encryption: KMS, Encryption SDK, SSM Parameter Store, IAM & STS
415. Encryption 101
✅ Exam Memory Hook

In Transit = while moving
🔹 Data protected with TLS/SSL as it travels between client ↔ AWS.
🔹 Example: HTTPS request to API Gateway or uploading to S3 securely.

Server-Side Encryption (SSE) = AWS encrypts for you
🔹 Data is encrypted after it arrives at AWS, before being written to disk.
🔹 AWS manages the heavy lifting (can use S3-SSE-S3, SSE-KMS, or SSE-C).
🔹 Example: S3 bucket with SSE-KMS enabled.

Client-Side Encryption = you encrypt before upload
🔹 Data is encrypted before leaving your machine/app.
🔹 You manage keys and decryption.
🔹 Example: Using AWS Encryption SDK to encrypt locally, then upload to S3.

⚡️ Exam tip:

If question says “don’t trust AWS with encryption keys” → Client-Side.

If question says “easiest/managed option” → Server-Side.

If question mentions network transfer security → In Transit.

0:32
31. AWS Other Services
437. Amazon Athena - Overview
Athena - Serverless query service to analyze data stored in amazon S3. Uses SQL to query the files in S3.

Commonly used with Amazon Quicksight for reporting/ dashboards.

Use cases - Business intelligence/ analytics/ reporting, analyse & query VPC flow logs, ELB logs, CloudTrail trails etc.

0:35
31. AWS Other Services
443. Amazon Macie
Macie - fully managed data security service uses machine learn and pattern matching . Protect PII

0:03
31. AWS Other Services
445. CloudWatch Evidently
📧 SES (Simple Email Service)
Used to send emails at scale (transactional/marketing).

Supports SMTP + SDK/API.

Verify domains/emails before sending.

Sandbox mode vs production.

🔔 SNS (Simple Notification Service)
Pub/Sub messaging (push-based).

Subscribers = SQS, Lambda, HTTP/S, Email, SMS.

Can do fan-out → 1 message → multiple subscribers.

FIFO topics exist (ordering, deduplication).

🔍 OpenSearch (Amazon OpenSearch Service)
Managed Elasticsearch fork.

Used for search and log analytics (often with CloudWatch or Kibana dashboards).

Index documents → run queries.

📊 Athena
Serverless SQL queries on S3 data.

Pay per query/amount of data scanned.

Schema defined via Glue Data Catalog.

0:36
31. AWS Other Services
445. CloudWatch Evidently
🔄 MSK (Managed Streaming for Apache Kafka)
Fully managed Kafka clusters.

Used for real-time event streaming.

Integrates with IAM and CloudWatch.

🔒 ACM (AWS Certificate Manager)
Issues and manages SSL/TLS certs (free for AWS resources).

Auto-renewal.

Integrated with CloudFront, ELB, API Gateway.



0:51
31. AWS Other Services
445. CloudWatch Evidently
🕵️ Macie
Security service → uses ML to detect PII/sensitive data in S3.

Helps with compliance (GDPR, HIPAA).

⚙️ AppConfig (part of Systems Manager)
Deploy and manage application configuration (not code).

Can roll out gradually, monitor with CloudWatch.

Useful for feature flags / dynamic configs.

📈 CloudWatch Application Insights / AppConfig Integration
CloudWatch AppInsights → monitors applications (like Java/.NET apps).

Helps detect anomalies, errors, bottlenecks.

AppConfig integrates with CloudWatch alarms for safe config rollouts.
