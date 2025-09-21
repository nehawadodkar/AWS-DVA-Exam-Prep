# 🔹 AWS Serverless Application Model (SAM) – DVA Notes

## What is SAM?
- Framework to define & deploy serverless applications using simplified CloudFormation templates.
- Resources: Lambda, API Gateway, DynamoDB, S3 triggers, etc.
- Allows local testing, packaging, deployment, and CI/CD integration.
- Extension of CloudFormation, not a separate service.

---

## Key SAM CLI Commands / API Calls

| Command / API           | Purpose                                                |
|-------------------------|--------------------------------------------------------|
| sam init                | Create a new serverless project/template             |
| sam build               | Build Lambda functions & dependencies locally        |
| sam local invoke        | Test a single Lambda function locally                |
| sam local start-api     | Emulate API Gateway + Lambda locally                 |
| sam package             | Package app artifacts for deployment (uploads to S3)|
| sam deploy              | Deploy packaged stack to AWS                          |
| sam delete              | Delete deployed stack/resources                      |
| sam sync                | Push local code changes to an existing stack without full redeploy |

### ⚡ Gotchas
- `sam local invoke / start-api` → local testing only.
- `sam sync` → requires existing deployed stack; not for first-time deployment.
- SAM always creates a CloudFormation stack.

---



---

# 🔹 AWS Deployment Strategies – DVA Notes

| Strategy               | Use Case                          | Key Features                                  | Services / Notes                              | Gotchas / Exam Hook                                      |
|------------------------|----------------------------------|-----------------------------------------------|-----------------------------------------------|---------------------------------------------------------|
| In-place Deployment    | Update existing Lambda function  | Old version replaced by new version           | Lambda console, SAM, CLI                       | Temporary downtime possible; old version overwritten   |
| Blue/Green Deployment  | Minimize downtime; test new version | Deploy new version alongside old; switch traffic | CodeDeploy, Lambda aliases, Elastic Beanstalk | Can roll back easily by redirecting traffic to old version |
| Canary Deployment      | Gradual rollout                  | Shift small % of traffic to new version, then increase | Lambda + CodeDeploy, API Gateway stage variables | Useful for testing small impact before full rollout    |
| All-at-once Deployment | Simple, small apps               | Deploy everything at once                      | CloudFormation, SAM, Lambda                    | Risky for critical production apps; full downtime if errors |
| Rolling Deployment     | Large fleet of EC2 / ECS         | Update instances in batches                     | Auto Scaling Groups, ECS services              | Requires batch size planning; avoids full outage       |
| Immutable Deployment   | Prevent downtime / rollback safely | Launch new instances, switch, terminate old   | Auto Scaling Groups, ECS, Beanstalk           | More resource-heavy; safer than in-place               |

### ⚡ Exam Tips
- **Lambda / Serverless:** Blue/Green or Canary preferred for production.  
- **EC2 / ECS:** Rolling or Immutable recommended for zero-downtime.  
- **SAM:** Typically uses **in-place** or **canary** via CodeDeploy.  
- Always know **risk vs downtime trade-off** for each strategy.

---

# 🔹 AWS Lambda – Detailed DVA Notes

## Function Structure
- **Handler:** Entry point of Lambda code  
  - Format: `file_name.method_name` (Python: `lambda_function.lambda_handler`)  
- **Runtime:** Python, Node.js, Java, Go, .NET, etc.
- **Execution Role:** IAM role Lambda assumes for accessing AWS services
- **Environment Variables:** Key-value pairs; can be encrypted with KMS
- **Memory & CPU:** Memory (128 MB – 10,240 MB); CPU scales with memory

---

## Deployment Package

| Type | Max Size | Notes |
|------|----------|------|
| Zip file (direct upload) | 50 MB | Includes your code + dependencies |
| Zip file (via S3)       | 250 MB | Upload larger code via S3 |
| Container image         | 10 GB  | Supports heavy dependencies, custom runtimes |

- **Handler file must be at root of zip** (unless using subdirectories + proper path)
- **Layers**: Shared libraries or code; max 5 layers, total 250 MB

---

## Execution & Timeout
- **Timeout:** 1 sec – 15 min (900 sec max)
- **Concurrency:** Reserved or provisioned concurrency controls number of simultaneous executions
- **Cold start:** First invocation latency, higher for VPC-attached or large functions

---

## Event Sources
- **Synchronous:** API Gateway, SDK calls, CloudWatch Logs → caller waits
- **Asynchronous:** S3, SNS, EventBridge → Lambda invoked in background
- **Polling:** SQS, DynamoDB Streams → Lambda polls events

---

## Security & Authorization
- Execution Role (IAM) → must have **least privilege** for AWS services
- Lambda Authorizer (API Gateway):
  - Types: **Token** (header/query) or **Request** (any request data)
  - Returns IAM policy (Allow/Deny)
  - Can cache results to reduce calls

---

## Deployment Tools
- **Console:** Quick, small functions
- **SAM / CloudFormation:** Infrastructure as code
- **Serverless Framework / CI/CD:** Automated pipelines
- **Container images:** Large apps or custom runtime requirements

---

## ⚡ Gotchas / Exam Points
- Max function size: **50 MB zip / 250 MB zip via S3 / 10 GB container**
- **Handler format** must match your code file + function
- Timeout **cannot exceed 15 min**
- Cold start → VPC + large packages = slower
- Reserved concurrency → may throttle invocations
- **SAM `sync`** only for existing deployed stacks; **cannot deploy first-time functions**

# 🔹 AWS Lambda Exceptions – DVA Notes (with Quick-Fix Hints)

| Exception Name                     | Cause / Scenario                                                | Exam Hook / Notes                                      | Quick-Fix / Hint                                           |
|-----------------------------------|-----------------------------------------------------------------|--------------------------------------------------------|------------------------------------------------------------|
| **InvalidParameterValueException** | Invalid memory, timeout, runtime, or other parameter values    | Common for misconfigured Lambda function settings    | Check memory, timeout, and runtime in function config     |
| **ResourceNotFoundException**      | Lambda function, alias, or layer not found                     | Check function name, alias, or layer ARN             | Verify ARN/name; ensure resource exists                   |
| **AccessDeniedException**          | IAM role lacks required permissions                             | Often seen with S3, DynamoDB, KMS access             | Update execution role with required permissions          |
| **KMSAccessDeniedException**       | Missing KMS permissions for DEK/Encrypt/Decrypt               | Happens when using encrypted env vars or SSE-KMS     | Add `kms:Decrypt` / `kms:GenerateDataKey*` to role       |
| **KMSDisabledException**           | KMS key disabled or deleted                                     | Encryption fails for Lambda env vars or S3 SSE-KMS   | Enable KMS key or select a different active key          |
| **ThrottlingException**            | Too many concurrent invocations or API requests                 | Use reserved/provisioned concurrency                 | Increase concurrency limits or retry after backoff       |
| **CodeStorageExceededException**   | Deployment package exceeds storage quota                        | Max 50 MB zip direct / 250 MB via S3 / 10 GB container | Reduce package size or use S3/container image           |
| **EC2SubnetLimitExceeded / ENILimitExceeded** | Lambda in VPC exceeds ENI / subnet limits                       | Happens for VPC-connected Lambdas                     | Reduce concurrency, increase subnet/IP allocation        |
| **ResourceConflictException**      | Attempt to update resource already being updated or in conflict | Rare, but possible with concurrent stack updates     | Retry later or avoid simultaneous updates               |
| **ServiceException / InternalServerError** | AWS internal error, retryable                                     | Often transient; retry recommended                   | Retry with exponential backoff                            |



## AWS CloudFormation Helper Scripts

AWS CloudFormation provides Python scripts to help set up and manage EC2 instances in your stack:

- **cfn-init** → Install software, create files, and start services based on resource metadata.  
- **cfn-signal** → Send signals to a CreationPolicy or WaitCondition to tell other resources that this one is ready.  
- **cfn-get-metadata** → Get metadata for a resource or a specific key.  
- **cfn-hup** → Watch for metadata updates and run custom hooks when changes happen.

## ⚡ SAM Deploy Gotchas

- `sam deploy` **automates build, upload to S3, and stack deployment**.  
- Must have **S3 bucket, stack name, and region configured** (via `--guided` or `samconfig.toml`).  
- Changes in local code/templates **won’t deploy** if `--guided` or config not updated.  
- For large apps, ensure **artifact size < S3/Lambda limits**.


## ⚡ Lambda Package Gotchas

- **Big Lambda** → Upload code to **S3** if package is large.  
- **Reusable/shared code** → Use **Layers** to keep function small and modular.  
- Max zipped size: 50 MB (direct), 250 MB (S3); max unzipped + layers: 250 MB.  
- A function can use **up to 5 layers**.

