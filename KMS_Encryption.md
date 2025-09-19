# 🔹 AWS KMS Encryption / Decryption – Cheat Sheet

---

## 1️⃣ Envelope Encryption (Large Data > 4 KB)

**Encrypting Data:**
```
Plaintext Data
      │
      ▼
GenerateDataKey (KMS) ─► Returns:
  ├─ Plaintext Data Key ─► Encrypt Data
  └─ Encrypted Data Key ─► Store with encrypted data
Encrypted Data + Encrypted Data Key stored
```

**Decrypting Data:**
```
Encrypted Data Key ─► KMS Decrypt ─► Plaintext Data Key ─► Decrypt Data
```

**Gotchas:**
- Always store **encrypted data key**, never plaintext.  
- Used for **S3 SSE-KMS, DynamoDB, large files**.

---

## 2️⃣ Direct Encryption (<4 KB)

**Encrypting Small Data:**
```
Plaintext Data
      │
      ▼
Encrypt (KMS, CMK) ─► Returns Encrypted Data
Store Encrypted Data
```

**Decrypting Small Data:**
```
Encrypted Data ─► Decrypt (KMS, CMK) ─► Returns Plaintext Data
```

**Gotchas:**
- Only for **small data (<4 KB)**.  
- KMS CMK encrypts data directly.

---

## 3️⃣ S3 SSE-KMS Upload / Download

**Upload (Encrypt):**
```
Client Upload ─► PutObject (SSE-KMS)
      │
      ▼
S3 Requests Data Key from KMS
      │
      ▼
KMS Returns:
  ├─ Encrypted Data Key
  └─ Plaintext Data Key
      │
      ▼
S3 Encrypts Object ─► Stores Object + Encrypted Data Key
```

**Download (Decrypt):**
```
S3 Retrieves Encrypted Data Key
      │
      ▼
KMS Decrypt ─► Plaintext Data Key
      │
      ▼
S3 Decrypts Object ─► Returns Data
```

**Gotchas:**
- S3 always uses **envelope encryption**, even if you just specify SSE-KMS.  
- Large files → **multipart uploads** also generate multiple data keys internally.  

---

## 4️⃣ Quick KMS Exam Gotchas

- **Data keys are always encrypted when stored.**  
- **Direct KMS encryption** → small data (<4 KB).  
- **Envelope encryption** → large files, S3 objects, DynamoDB.  
- **SSE-KMS in S3** always uses envelope encryption behind the scenes.  
- Always ensure **IAM permissions** for KMS:
  - `kms:Decrypt`, `kms:GenerateDataKey*` for uploads/downloads.  
  - Missing `kms:Decrypt` can cause **Access Denied** for large files.  
- Maximum data KMS can encrypt directly = **4 KB**.  

---

## 5️⃣ Quick Reference Table – KMS vs S3

| Scenario                    | Encryption Type        | KMS Role / Key Used                 |
|-------------------------------|----------------------|-----------------------------------|
| Small data (<4 KB)           | Direct               | CMK                                |
| Large data / S3 objects      | Envelope             | CMK + data key                     |
| S3 SSE-KMS                    | Envelope             | CMK + data key                     |
| DynamoDB server-side KMS      | Envelope             | CMK + data key                     |
