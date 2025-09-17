# Encryption Notes - DVA Exam Quick Revision

## AES-256
- **Advanced Encryption Standard** with **256-bit symmetric key**.  
- Same key used for **encryption & decryption**.  
- Used in **S3 (SSE-S3/SSE-KMS), EBS, RDS**.  

**⚡ Gotcha:** AES-256 = symmetric, **do not confuse with asymmetric keys**.

---

## Envelope Encryption
- **Two-layer encryption**:
  1. **Data Key (DEK):** encrypts the actual data (fast, per-object key)  
  2. **Key Encryption Key (KEK):** encrypts the DEK (strong, centrally managed, stored in **KMS**)  

**How it works:**  
- Data → encrypted with DEK → ciphertext  
- DEK → encrypted with KEK → stored safely  
- Decrypt: KEK → DEK → data  

**Benefits:**  
- Secure key management, scalable, allows DEK rotation  
- Used in **S3 SSE-KMS**, **EBS**, **RDS encryption**

**⚡ Gotcha / Mnemonic:**  
- Envelope Encryption = **“Lock the key, then lock the data”**  
- KEK = stored in **KMS**, never leaves unencrypted  
- DEK = per-object, encrypted by KEK  

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

**⚡ Gotcha:** *Data is never encrypted directly with the KMS key — only with DEKs.*

Mnemonic: **Data with DEK → DEK with KEK**.

---

## How Keys Are Used
- **Encrypt API call:**  
  - Send plaintext to KMS (small amounts only, up to 4KB).  
  - Not scalable for large data.  
- **GenerateDataKey API call:**  
  - KMS returns **plaintext DEK + encrypted DEK**.  
  - Use plaintext DEK locally to encrypt big data, then discard it.  
  - Store encrypted DEK with your data.  

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


