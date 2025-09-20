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




# 🔹 AWS Deployment Strategies – DVA Notes

| Strategy | Use Case | Key Features | Services / Notes | Gotchas / Exam Hook |
|----------|---------|-------------|-----------------|------------------|
| **In-place Deployment** | Update existing Lambda function or app | Old version replaced by new version | Lambda console, SAM, CLI | Temporary downtime possible; old version overwritten |
| **Blue/Green Deployment** | Minimize downtime; test new version | Deploy new version alongside old; switch traffic | CodeDeploy, Lambda aliases, Elastic Beanstalk | Can roll back easily by redirecting traffic to old version |
| **Canary Deployment** | Gradual rollout | Shift small % of traffic to new version, then increase | Lambda + CodeDeploy, API Gateway stage variables | Useful for testing small impact before full rollout |
| **All-at-once Deployment** | Simple, small apps | Deploy everything at once | CloudFormation, SAM, Lambda | Risky for critical production apps; full downtime if errors |
| **Rolling Deployment** | Large fleet of EC2 / ECS | Update instances in batches | Auto Scaling Groups, ECS services | Requires batch size planning; avoids full outage |
| **Immutable Deployment** | Prevent downtime / rollback safely | Launch new instances, switch, terminate old | Auto Scaling Groups, ECS, Beanstalk | More resource-heavy; safer than in-place |

---

### ⚡ Exam Tips
- **Lambda / Serverless:** Blue/Green or Canary preferred for production.  
- **EC2 / ECS:** Rolling or Immutable recommended for zero-downtime.  
- **SAM:** Typically uses **in-place** or **canary** via CodeDeploy.  
- Always know **risk vs downtime trade-off** for each strategy.

