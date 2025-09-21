# ⚡ AWS Lambda – DVA Exam Notes

Comprehensive notes on AWS Lambda concepts for deployment, automation, and exam prep.

---

## 1️⃣ Lambda Versions
- **Immutable snapshot** of code + configuration.  
- `$LATEST` → mutable, changes on each update (⚠️ not safe for production).  
- Each `publish-version` creates a new version number.  
- Use versions for **release history and rollback**.

✅ Gotcha: Always pair versions with **aliases** for stable references.

---

## 2️⃣ Lambda Aliases
- **Named pointers to versions** (e.g., `dev`, `test`, `prod`).  
- Can shift traffic between versions (e.g., 90% v1, 10% v2).  
- Enable **blue/green** or **canary deployments** with CodeDeploy.  

✅ Gotcha: `$LATEST` cannot be used with traffic shifting.

---

## 3️⃣ Lambda Layers
- Package and share **common code/libraries** across functions.  
- Each function can use up to **5 layers**.  
- Reduces package size, centralizes dependency management.  
- **Size limits**: 50 MB zipped, 250 MB unzipped.  

✅ Gotcha: Layers are **versioned**; updating requires re-publishing.

---

## 4️⃣ Environment Variables
- Key–value pairs available at runtime.  
- Use for config (DB names, API keys, stage values).  
- Can be **encrypted with KMS**.  
- Different per environment (`dev`, `prod`).  

✅ Gotcha: Don’t store secrets directly → use Secrets Manager or SSM Parameter Store.

---

## 5️⃣ Execution Context
- The **sandbox environment** for Lambda code.  
- Includes: runtime, `/tmp` storage (512 MB), env vars.  
- **Warm start**: context reused → faster execution.  
- **Cold start**: new context created → slower first run.  

✅ Gotcha: `/tmp` persists only for warm starts, not cold.

---

## 6️⃣ Concurrency
- **Default Concurrency**: Shared pool across account.  
- **Reserved Concurrency**: Limits a function to X concurrent executions.  
- Prevents noisy-neighbor issues.  
- Helps guarantee availability for critical functions.  

✅ Gotcha: Setting reserved concurrency = hard cap → function throttles above limit.

---

## 7️⃣ Deployment Packages
- Package options:  
  - **Zip upload**: 50 MB max (zipped), 250 MB max (unzipped + layers).  
  - **Container images**: up to **10 GB** via ECR.  
- Use SAM/CloudFormation/CLI for deployment.  

✅ Gotcha: For large apps → use container images, not zip.

---

## 8️⃣ Permissions
- **Execution Role (IAM Role)** → what Lambda can access (S3, DynamoDB, etc.).  
- **Resource-based Policy** → who/what can invoke Lambda (API Gateway, S3 event, ALB).  

✅ Gotcha: Most “Lambda cannot access resource” exam Qs = wrong role or missing policy.

---

## 9️⃣ Destinations
- For **async invocations** (success/failure).  
- Options:  
  - **SQS**  
  - **SNS**  
  - **EventBridge**  
  - Another Lambda  

✅ Gotcha: Only for async flows (not sync).

---

## 🔟 Dead Letter Queues (DLQ)
- For failed **async events**.  
- Supported targets: **SQS** or **SNS**.  
- Helps with debugging and retry later.  

✅ Gotcha: DLQ ≠ Destinations. DLQ = failures only, Destinations = success + failure.

---

## 1️⃣1️⃣ Timeouts & Memory
- **Timeout**: max 15 minutes.  
- **Memory**: 128 MB → 10 GB.  
- CPU allocated proportionally to memory.  

✅ Gotcha: More memory = more CPU + network bandwidth.

---

## 1️⃣2️⃣ Deployment Strategies
- Use **Aliases + CodeDeploy**:  
  - **Canary** (e.g., 10% traffic, then 90%).  
  - **Linear** (gradually increase).  
  - **All-at-once**.  

✅ Gotcha: Safe deployments require **aliases**.

---

# ⚡ Quick Memory Hooks

- **Version** = Snapshot.  
- **Alias** = Environment pointer.  
- **Layer** = Shared library shelf.  
- **Env Var** = Settings bag.  
- **Execution Context** = Sandbox (warm vs cold).  
- **Concurrency** = Traffic lane size.  
- **DLQ** = Failure mailbox.  
- **Destinations** = Where async results go.  
- **Package** = Zip (250 MB) or Image (10 GB).  
- **Timeout** = 15 min max.
- 
## 1️⃣3️⃣ Lambda Handler
- Entry point for Lambda execution; receives `event` (input) and `context` (runtime info).  
- Sync → return value sent to caller; Async → use Destinations/DLQ.  
- Handler name must match runtime; avoid heavy init outside handler to reduce cold starts.
