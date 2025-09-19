# 🛡️ AWS DVA-C02 Exam Notes – Domain 2: Security

This section covers **Authentication & Authorization, Encryption, and Sensitive Data Management**.  
Use these notes for **revision + last-day exam prep**.

---

## 📌 Task 1: Authentication & Authorization

### 🔑 Core Concepts

| Concept | Explanation | Exam Tip |
|---------|-------------|----------|
| **Identity Federation** | Use external IdPs (SAML, OIDC, Google, Facebook, Cognito) → IAM Role via STS | *Think: “Don’t create IAM users for everyone.”* |
| **Bearer Tokens** | Temporary credentials (JWT, OAuth, STS) | JWT = Cognito, OAuth; STS = AWS temporary creds |
| **Amazon Cognito** | - **User Pools** → Auth, user directory, JWT tokens <br> - **Identity Pools** → Temporary AWS creds (via STS) | *Memory Trick*: **User = Users sign in**, **Identity = Identities get AWS creds** |
| **Policies** | - **Resource-based**: Attached to resource (S3, Lambda) <br> - **Service policy**: Service roles <br> - **Principal policy**: IAM user/role/group | Resource-based supports **cross-account access** |
| **RBAC (Role-Based Access Control)** | Permissions tied to IAM role | Developers **assume roles** in code |
| **ACLs** | Legacy (S3 object ACL, VPC NACLs) | Used rarely; exam may test difference vs IAM |
| **Least Privilege** | Always restrict to minimum permissions | Frequent exam keyword |
| **Managed Policies** | - **AWS-managed**: Created + updated by AWS <br> - **Customer-managed**: Created by you | *Gotcha*: Custom = more control but more mgmt overhead |

---

### 🛠️ Skills
- Implement **federated login** with Cognito or IAM + IdP.  (IdP = Identity Provider = SAML IdP (corporate SSO, active directory), aAuth IdP (google, fb etc) )
- Secure apps with **JWT** / bearer tokens.  
- Configure **programmatic access** (IAM roles > access keys).  
- Use **AssumeRole** for temporary access.  
- Define **who (principal)** can **do what (action)** on **which resource**.

---

## 📌 Task 2: Encryption

### 🔑 Core Concepts

| Concept | Explanation | Exam Tip |
|---------|-------------|----------|
| **Encryption at Rest** | Data stored encrypted (S3, EBS, RDS) | Default SSE-S3 is AES-256 |
| **Encryption in Transit** | TLS/SSL → HTTPS | ACM issues certs for ELB, CloudFront, API gateway |
| **AWS ACM / Private CA** | ACM: public certs <br> Private CA: internal org certs | ACM certs free for supported services |
| **KMS Key Types** | - **AWS-managed key (aws/service-name)** → Auto-rotated yearly <br> - **Customer-managed CMK** → Full control, policies, rotation | *Gotcha*: Customer-managed = optional rotation |
| **Client-side vs Server-side Encryption** | - Client-side → App encrypts before sending <br> - Server-side → AWS encrypts after receiving | SSE options: SSE-S3, SSE-KMS, SSE-C |
| **Key Rotation** | Enabled by default for AWS-managed <br> Optional for CMKs | Exam may ask: default = **1 year** |

---

### 🛠️ Skills
- Use **KMS** to encrypt/decrypt data.  
- Generate **SSH keys** for EC2/dev use.  
- Share encrypted data across accounts (via **KMS key policy**).  
- Enable/disable **key rotation** as per compliance.

---

## 📌 Task 3: Managing Sensitive Data

### 🔑 Core Concepts

| Concept | Explanation | Exam Tip |
|---------|-------------|----------|
| **Data Classification** | PII (Personal Identifiable Info), PHI (Health Info) | Protect with encryption + restricted access |
| **Environment Variables** | Don’t hardcode secrets; use encrypted env vars | Encrypt Lambda env vars with KMS |
| **Secrets Management** | - **Secrets Manager** → Auto rotation, DB/API creds <br> - **SSM Parameter Store** → Config/secrets (Standard = free, Advanced = $$) | *Memory Trick*: Secrets Manager = rotation; Parameter Store = config |
| **Secure Credential Handling** | Never store creds in code/git <br> Use IAM roles not static keys | Frequent exam trap |

---

### 🛠️ Skills
- Encrypt **Lambda environment variables**.  
- Use **Secrets Manager / Parameter Store** in code.  
- **Sanitize logs** (don’t log creds/PII).  
- Apply **least privilege** for secrets access.

---

## ⚠️ Exam Gotchas & Memory Traps

- **Cognito User Pool vs Identity Pool** → User = sign-in, Identity = AWS creds.  
- **STS tokens are temporary** → always short-lived + scoped.  
- **Resource-based policies allow cross-account access** (unlike IAM policies).  
- **Default KMS key rotation = 1 year** (only for customer-managed if enabled).  
- **Secrets Manager costs $$, Parameter Store Standard = free**.  
- **Never put creds in EC2 user data or Lambda code**.  
- **ACM certs** work only with ELB, CloudFront, API Gateway (not EC2 directly).  

---

## 🧠 Quick Memory Tricks

- **Think: "U → Users, I → Identities"** for Cognito pools.  
- **RBAC = Roles Bring Access Control**.  
- **Bearer = “I bear a token” → temporary proof**.  
- **KMS keys**: AWS = auto, Customer = manual (control).  
- **Secrets Manager = Rotation, Parameter Store = Configuration**.

# STS vs Cognito User Pool – Minimalist Exam Notes

| Feature        | STS (Security Token Service)                  | Cognito User Pool                   |
|----------------|-----------------------------------------------|--------------------------------------|
| Purpose        | Issues **temporary AWS credentials**          | Handles **user authentication** (sign-up/sign-in) |
| What it gives  | Access Key, Secret Key, Session Token         | JWT tokens (ID, Access, Refresh)    |
| Focus          | **Authorization** (access to AWS resources)   | **Authentication** (who the user is) |
| Validity       | Short-term (15 min – 12 hrs)                  | Long-lived (refresh token up to 30 days) |
| Typical use    | Cross-account access, role assumption         | App login (web/mobile, social, SAML, OIDC) |

---

🧠 **Memory Hack**:  
- **User Pool = Users login**  
- **STS = Short-Term AWS creds**

- # Cognito Sync vs AppSync – Minimalist Notes

| Feature        | Cognito Sync (Legacy)                        | AppSync                                |
|----------------|----------------------------------------------|-----------------------------------------|
| Purpose        | Sync **user data/preferences** across devices | Build **GraphQL APIs** for apps          |
| What it syncs  | Key-value pairs (like settings, game scores)  | Any app data from DynamoDB, Lambda, RDS, etc. |
| Scope          | **Per-user** data                            | **App-wide** data/API layer             |
| Backend        | Uses Cognito Identity + datasets             | Managed **GraphQL service**             |
| Status         | **Legacy / deprecated** → replaced by AppSync | Actively supported, modern choice        |

---

🧠 **Memory Hack**:  
- **Cognito Sync → “tiny sync” (per-user prefs).**  
- **AppSync → “app sync” (whole app’s data via GraphQL).**



---


