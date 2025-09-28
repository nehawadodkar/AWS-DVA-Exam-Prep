# CloudFront – Origin Access Control (OAC)

- **OAC:** A secure way for CloudFront to access your S3 bucket or custom origin.  
- **Purpose:** Ensures **only CloudFront** can fetch from origin → block direct access.  
- **Replaces:** Origin Access Identity (OAI) with more features (sigv4 signing).  
- **Works with:** S3, ALB, or custom origins.

- # AWS X-Ray – Namespace, Subsegment, Remote Subsegment

- **Segment:** Trace of a single request through your app.  
- **Subsegment:** Breaks a segment into smaller units (e.g., function calls, DB query).  
- **Remote Subsegment:** Represents calls to **downstream services** (e.g., DynamoDB, S3, HTTP).  
- **Namespace:** Groups remote subsegments by service type (e.g., `aws`, `remote`).
    - aws - for aws SDK calls like ones to S3. dynamo DB etc
    - remote - for non aws calls like  http etc.   

💡 **Gotcha:**  
- Use **subsegments** for internal work.  
- Use **remote subsegments** + **namespace** when calling AWS services or external APIs.

- # CloudFormation Drift Detection  

-  Checks if stack resources match the template; flags changes as `DRIFTED`.  
-  Manual or API-triggered (not automatic).

**Cognito Adaptive Auth:** Detects suspicious logins (IP/device/location) and adds MFA or blocks access.  


# 🔐 Amazon S3 SSE Headers Cheat Sheet

| SSE Type  | Who Manages Keys? | Upload (PUT/POST) Headers | Download (GET) Headers | Notes |
|-----------|-------------------|---------------------------|-------------------------|-------|
| **SSE-S3** | AWS (S3) | `x-amz-server-side-encryption: AES256` | None | Easiest, fully managed by S3. |
| **SSE-KMS** | AWS KMS | `x-amz-server-side-encryption: aws:kms` <br> `x-amz-server-side-encryption-aws-kms-key-id: <KMS-Key-ID-or-ARN>` *(optional)* <br> `x-amz-server-side-encryption-context: <Base64-JSON>` *(optional)* | None | Supports audit logging & fine-grained IAM control. |
| **SSE-C** | Customer | `x-amz-server-side-encryption-customer-algorithm: AES256` <br> `x-amz-server-side-encryption-customer-key: <Base64-256-bit-key>` <br> `x-amz-server-side-encryption-customer-key-MD5: <Base64-MD5-of-key>` | Same 3 headers required | AWS never stores the key. You must supply it for every request. Losing the key = permanent data loss. |

  

