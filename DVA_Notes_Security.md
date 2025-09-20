# Encryption Notes - DVA Exam Quick Revision

## AES-256
- **Advanced Encryption Standard** with **256-bit symmetric key**.  
- Same key used for **encryption & decryption**.  
- Used in **S3 (SSE-S3/SSE-KMS), EBS, RDS**.  

**⚡ Gotcha:** AES-256 = symmetric, **do not confuse with asymmetric keys**.

---

## 🔐 Envelope Encryption

- **Two-layer encryption:**  
  - **Data Key (DEK):** encrypts actual data (fast, per-object key).  
  - **Key Encryption Key (KEK/CMK):** encrypts the DEK (stored in KMS).  

- **How it works (AWS API calls):**  
  1. App calls **KMS → `GenerateDataKey`**  
     - Returns: **Plaintext DEK** + **Encrypted DEK**.  
  2. **Plaintext DEK** → used in memory to encrypt data (e.g., with AES) → discarded.  
  3. Store **Encrypted DEK** with the ciphertext.  
  4. To decrypt:  
     - App calls **KMS → `Decrypt`** with Encrypted DEK.  
     - KMS uses KEK to return **Plaintext DEK**.  
     - App uses DEK to decrypt the data.  

- **Benefits:**  
  - Secure key management.  
  - Scalable (per-object DEK).  
  - DEK rotation possible.  
  - Used in **S3 SSE-KMS, EBS, RDS encryption**.
  - 

- **Gotcha / Mnemonic:**  
  - *“Lock the key, then lock the data.”*  
  - KEK = in KMS, never leaves unencrypted.  
  - DEK = temporary plaintext; only **encrypted DEK** is stored.  
  - ⚡ **Exam Tip:** KMS can only directly encrypt ≤ **4 KB** → larger data must use **Envelope Encryption**.


---

**Quick Recall Table:**

| Term | Purpose | Stored / Managed | Exam Tip |
|------|---------|-----------------|----------|
| AES-256 | Symmetric encryption of data | Key known to encrypt/decrypt | Fast, secure, symmetric |
| DEK | Encrypts actual data | Stored with ciphertext | Per-object key, fast |
| KEK | Encrypts DEK | KMS / secure service | Strong, central key |


# Cross-Account Access

**Cross-account access** = Allowing resources or users in **one AWS account** to access resources in **another AWS account** securely.  

- Achieved using **IAM roles with trust policies** (not by sharing long-term credentials).  
- Common use cases:  
  - Centralized logging account accessing logs from other accounts (CloudWatch, S3).  
  - CI/CD account deploying resources into multiple target accounts.  
  - Sharing S3 buckets, KMS keys, DynamoDB tables, etc. across accounts.  

**⚡ Gotcha:** Always use **IAM roles + trust policies**, not IAM users/keys, for cross-account setups.


# AWS Key Management Service (KMS) – Exam Notes

## What is KMS?
- Managed service for creating, controlling, and using **encryption keys**.
- Provides **Customer Managed Keys (CMKs)**, now called **KMS Keys**.
- Keys are stored in **FIPS 140-2 validated HSMs** (Hardware Security Modules).
- Integrates with most AWS services (S3, EBS, RDS, DynamoDB, Lambda, etc.).

---

## Types of Keys
- **AWS Managed Keys:** Auto-created, free, one per service per region.
- **Customer Managed Keys (CMK/KMS Key):** Created & managed by you, full control (rotation, policies).
- **Customer Provided Keys:** Bring your own key material.
- **Data Key (DEK):** Temporary symmetric key to encrypt data (used in envelope encryption).

---

## Key Concepts
- **Symmetric Keys:** Single key for encrypt/decrypt. Default, widely used in AWS.
- **Asymmetric Keys:** Public/private key pairs (e.g., for digital signatures, RSA).
- **Key Policies:** JSON documents that define **who can use/manage keys**.
- **Automatic Key Rotation:**  
  - AWS managed keys → rotated every 3 years.  
  - Customer managed keys → optional yearly rotation.  

---

## Envelope Encryption
1. Generate a **Data Key (DEK)**.  
2. Encrypt plaintext data with DEK.  
3. Encrypt DEK with **KMS Key (KEK)**.  
4. Store encrypted DEK alongside ciphertext.  

**⚡ Gotcha:** Data is *usually* encrypted via DEKs (Envelope Encryption).  
KMS can directly encrypt small data (≤ 4 KB) with `Encrypt`, but this is rare in practice.

Mnemonic: **Data with DEK → DEK with KEK**.

---

## How Keys Are Used (Envelope Encryption)

- **Encrypt API call:**  
  - Send plaintext to KMS (small amounts only, ≤ 4 KB).  
  - Not scalable for large data.

- **GenerateDataKey API call:**  
  - KMS returns **plaintext DEK + encrypted DEK**.  
  - Use plaintext DEK locally to encrypt big data → discard plaintext.  
  - Store **encrypted DEK** with ciphertext.

- **Decrypt API call:**  
  - Send **encrypted DEK** to KMS → KMS decrypts with KEK (CMK).  
  - KMS returns **plaintext DEK** → use it to decrypt your data locally.  
  - Discard plaintext DEK after use.

---

## Integration Examples
- **S3 SSE-KMS:** Data encrypted with DEK, DEK encrypted by KMS CMK.
- **EBS Encryption:** Volume data encrypted via KMS.
- **Lambda Env Vars:** Encrypted at rest using KMS.

---

## Gotchas ⚡
- **Plaintext key exposure:** DEK plaintext must be discarded after use.
- **KEK (CMK) never leaves KMS in plaintext** – all ops happen inside HSM.
- **SSE-C (Customer-Provided Key):** AWS does not store your key; if lost, data is unrecoverable.
- **KMS Encrypt API limit:** Only small data (≤4KB). Use DEK for large files.
- **Cross-Account:** Use IAM roles + key policy for access, never long-term keys.
- **Key Deletion:** Scheduling required (min 7 days). Immediate delete not allowed.
- **Rotation:** Auto for AWS-managed; opt-in for customer-managed.
- **Asymmetric keys:** Limited usage (signing/verification, not bulk data encryption).

---

## Exam Tips
- KMS = **regional service** (keys don’t move between regions).  
- For **compliance/strict security**, use **Customer Managed Keys**.  
- Envelope encryption = must-know scenario.  
- For **performance** → always encrypt large data with **DEKs**, not directly with CMKs.  
- If a question says “strict policy, manage your own keys” → Answer = Customer Managed CMK.  

# 🔑 AWS STS (Security Token Service) – DVA Exam Notes

---

## 📌 What is STS?
- **AWS STS = Security Token Service**
- Issues **short-term, temporary security credentials**:
  - **Access Key ID**
  - **Secret Access Key**
  - **Session Token**
  - **Expiration** (15 min – 12 hrs depending on API)
- Removes the need for **long-term IAM users/keys**.

---

## 📌 When is STS used?

| Use Case | Example | API Call |
|----------|---------|----------|
| **Cross-Account Access** | Assume role in another AWS account | `AssumeRole` |
| **Federated Access (SAML)** | Corporate directory via Active Directory, Okta | `AssumeRoleWithSAML` |
| **Federated Access (OIDC)** | Login via Google, Facebook, Amazon Cognito | `AssumeRoleWithWebIdentity` |
| **Temporary Elevated Access** | MFA-protected API calls | `GetSessionToken` |
| **Custom Federation** | Temporary creds for non-IAM users | `GetFederationToken` |

---

## 📌 Key STS API Calls

- `AssumeRole` → Assume IAM role (same or cross-account).  
- `AssumeRoleWithSAML` → SAML-based federation.  
- `AssumeRoleWithWebIdentity` → OIDC-based federation (Cognito, Google).  
- `GetSessionToken` → Temporary creds, often with MFA.  
- `GetFederationToken` → Temporary creds for federated users.  

---

## 📌 Exam Gotchas

- **Always temporary**: STS credentials are short-lived.  
- **Cognito Identity Pools** → Use STS to give AWS creds.  
- **Cross-account access** → Usually involves STS + resource-based policy.  
- **Best practice**: Prefer STS + IAM roles over long-term access keys.  

---

## 🧠 Memory Trick
**STS = Short-Term Security** → “I give you temporary keys, not permanent ones.”


# 🔐 S3 Encryption & SigV4 – Exam Refresher

## 1. What SigV4 Does (and Doesn’t Do)
- **SigV4 (with salted HMAC)** = Signs API requests to prove identity + integrity.  
- **It does NOT decrypt objects.**  
- Decryption depends on the S3 encryption mode.

---

## 2. S3 Encryption Modes (Who supplies the key? Who decrypts?)

| Mode      | Who supplies key? | Where is key stored? | Who decrypts on GET? | Exam Tip |
|-----------|------------------|----------------------|-----------------------|----------|
| **SSE-S3** (S3 managed keys) | AWS (auto-managed) | In S3 (AWS-managed) | S3 automatically | “Easiest mode – just works if you have S3 perms.” |
| **SSE-KMS** (KMS keys) | AWS KMS CMK (AWS or customer-managed) | In KMS | S3 (after calling KMS `Decrypt`) | Need `kms:Decrypt` + IAM perms. Watch cross-account gotchas. |
| **SSE-C** (Customer-provided) | You (must supply key in headers for PUT & GET) | Not stored by S3 | S3 (derives encryption key using supplied key - HMAC-style derivation process) | If you lose key → data unrecoverable. Must send key every time. |
| **Client-side** | You (encrypt before upload) | You manage outside AWS | You (decrypt after download) | AWS only stores ciphertext. |

---

## 3. SSE-C Deep Dive
- Upload: `Object + key` → S3 salts/derives an internal key → encrypts → discards your key.  
- Download: You send **same key** → S3 re-derives internal key → decrypts object.  
- Wrong/missing key = object cannot be decrypted.  
- **AWS never stores your raw key.**

---

## 4. Quick Mnemonics
- **SSE-C** → *Customer supplies key every time*.  
- **SSE-KMS** → *KMS decides if you can decrypt*.  
- **SSE-S3** → *S3 takes care of everything*.  
- **Client-side** → *Client does all the work*.  

---

## 5. Exam Gotchas
- SigV4 = *signing*, not *encryption*.  
- SSE-KMS requires BOTH:  
  - IAM policy allowing S3 access.  
  - KMS key policy allowing `kms:Decrypt`.  
- SSE-C: AWS **never stores your key** → lose it = lose data.  
- SSE-S3: Simplest, default server-side encryption.  
- Client-side: AWS is blind to plaintext.  

## 🔒 SSL/TLS Certs – Exam Gotchas

### ✅ Import Options
- **ACM** → Preferred, free certs + import 3rd-party, auto-renew, works with ALB/CF/APIGW  
- **IAM Cert Store** → Legacy, only if ACM not supported, supports 3rd-party, no auto-renew  

### ❌ Not for Import
- CloudFront → can only *use* ACM/IAM certs  
- Cognito → identity only  
- S3 → not usable for certs  

### ⚡ Quick Recall
- 3rd-party cert → **ACM (best) or IAM (legacy)**  
- ACM = Managed / Auto-renew  
- IAM = Fallback / Manual



### ⚡ Exam Gotcha
- **AccessDenied on large uploads** → usually missing `kms:Decrypt` or `kms:GenerateDataKey*`   ⚡ ⚡ ⚡ **(Its decrypt and not encrypt !!! )**  ⚡ ⚡ ⚡
- Small files may succeed because only 1 KMS call is needed  
- Always check **IAM + CMK key policy** permissions for multipart uploads

---
## 📌 S3 Server-Side Encryption Headers – Quick Reference

| Header                                      | Value / Purpose                     | Used In Scenario                                       | Notes / Gotchas |
|--------------------------------------------|------------------------------------|-------------------------------------------------------|----------------|
| `x-amz-server-side-encryption`             | `AES256`                            | SSE-S3 (Amazon-managed keys)                          | Ensures encryption, but not KMS |
| `x-amz-server-side-encryption`             | `aws:kms`                           | SSE-KMS (AWS KMS-managed keys)                        | Only specifies KMS, may use default CMK |
| `x-amz-server-side-encryption-aws-kms-key-id` | KMS Key ID or ARN                  | SSE-KMS with a **specific KMS key**                  | Required if bucket policy enforces a specific key |
| `x-amz-server-side-encryption-context`     | Custom key-value context           | Optional advanced SSE-KMS scenarios                  | Can enforce encryption context checks |
| `Content-Length`                            | Object size in bytes                | Useful in PUT requests for S3                        | Not an SSE header, but sometimes referenced in policies |
| `x-amz-meta-*`                              | User-defined metadata               | Optional metadata on objects                          | Can store info about encryption, owner, etc. |

### ⚡ Exam Gotchas
- **PUT** request → programmatic upload; **must match bucket policy headers**  
- **POST** request → browser form upload; policy can include conditions on headers too  
- **x-amz-server-side-encryption** alone → allows SSE-S3 or SSE-KMS (default CMK)  
- **x-amz-server-side-encryption-aws-kms-key-id** → enforces use of a specific KMS key


## 🔹 Lambda Authorizer (API Gateway)

- **Purpose:** Custom authentication and authorization for API Gateway requests.
  - Lets you implement logic beyond standard IAM roles or API keys.
  
- **Types:**
  - **Token Authorizer:** Receives a single token (e.g., from `Authorization` header or query string). Good for JWT or OAuth tokens.
  - **Request Authorizer:** Receives full request context (headers, query params, stage variables, etc.) for more complex access decisions.

- **How it works:**
  1. Client sends request to API Gateway.
  2. API Gateway calls Lambda Authorizer.
  3. Lambda returns an **IAM policy** (`Allow` or `Deny`) for the request.
  4. API Gateway enforces the policy and forwards request if allowed.

- **Features:**
  - Can **cache results** to reduce Lambda calls and improve performance.
  - Returns context values (`context` object) that can be passed to downstream Lambda.

- **Exam Gotchas:**
  - Must return a valid **policy with `Effect` and `Resource`**.
  - If invalid token → return `Unauthorized`; otherwise API returns 403.
  - Caching misconfiguration may allow invalid access if not careful.


---

## 🔹 CORS
- Lets **browser apps from other domains** access your API/S3.
- Browser sends **OPTIONS preflight**; server must return **CORS headers**.
- Missing headers → browser blocks request (client-side).
- Must configure in **API Gateway or S3**.

## 🔗 Hotlinking Prevention

- **Hotlinking:** Other sites link to your S3 files → you pay bandwidth.  
- **Fix:** Use **CloudFront + Signed URLs/Cookies**.  
- **Gotcha:**  
  - S3 Pre-signed URL → object-level temp access.  
  - CloudFront Signed URL/Cookie → best for hotlinking.



## 🛡️ Trust Policies (IAM Roles)

- **What:** JSON policy attached to a **Role**, defines **who can assume the role**.  
- **Action:** Always includes `sts:AssumeRole` (or `AssumeRoleWithWebIdentity`, `AssumeRoleWithSAML`).  
- **Principal:** Specifies *who/what* can assume (e.g., `ec2.amazonaws.com`, another AWS account, IAM user).  

### 🔑 Trust vs. Permissions Policy
- **Trust Policy:** *Who can wear the jacket* (assume role).  
- **Permissions Policy:** *What the jacket lets you do* (role’s rights once assumed).  


⚡ **Exam Gotchas**

- Role won’t work without both:
- Trust policy → who can assume.
- Permissions policy → what role can do.
- Cross-account access: trust policy must allow external account’s IAM principal.


- **Cross-account access (AWS → AWS):** Use **IAM Roles + Trust Policies**.  
- **External access (on-prem / local machine / external apps):** Use **long-term AWS access keys** (stored in `~/.aws/credentials` or env vars).  

⚡ Gotcha: For on-prem/external, if possible use federation + AssumeRoleWithSAML/WebIdentity instead of static keys (best practice, avoids hardcoding).
