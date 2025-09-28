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
  

