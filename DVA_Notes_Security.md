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

