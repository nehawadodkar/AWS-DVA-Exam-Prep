# 🔹 AWS Serverless Application Model (SAM) – DVA Notes

## What is SAM?
- Framework to define & deploy **serverless applications** using **simplified CloudFormation templates**.
- Resources: Lambda, API Gateway, DynamoDB, S3 triggers, etc.
- Allows **local testing**, packaging, deployment, and CI/CD integration.
- **Extension of CloudFormation**, not a separate service.

---

## Key SAM CLI Commands / API Calls

| Command / API | Purpose |
|---------------|---------|
| `sam init` | Create a new serverless project/template |
| `sam build` | Build Lambda functions & dependencies locally |
| `sam local invoke` | Test a single Lambda function locally |
| `sam local start-api` | Emulate API Gateway + Lambda locally |
| `sam package` | Package app artifacts for deployment (uploads to S3) |
| `sam deploy` | Deploy packaged stack to AWS |
| `sam delete` | Delete deployed stack/resources |
| `sam sync` | Push local code changes to an **existing stack** without full redeploy |

⚡ **Gotchas:**
- `sam local invoke` / `start-api` → **local testing only**.  
- `sam sync` → **requires existing deployed stack**; not for first-time deployment.  
- SAM **always creates a CloudFormation stack**.

---

## SAM Deployment Flow

```text
Local Dev
   │
   ▼
sam init ──> Project template
   │
   ▼
sam build ──> Build Lambda + dependencies
   │
   ▼
sam local invoke / sam local start-api ──> Test locally
   │
   ▼
sam package ──> Package artifacts to S3 (optional, can skip with deploy)
   │
   ▼
sam deploy ──> Deploy CloudFormation stack to AWS
   │
   ▼
sam sync ──> Incremental updates to existing stack (optional)
   │
   ▼
Production / Dev stack updated
