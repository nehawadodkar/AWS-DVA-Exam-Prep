# 🚀 AWS DVA Exam – Deployment Toolkit (Task 3: Automate Deployment Testing)

This guide covers all **AWS services relevant to deployment** for the Developer Associate Exam.  
Organized by category for faster revision: IaC, CI/CD, Compute, Containers, Networking, Security.  

---

## 1️⃣ Infrastructure as Code (IaC) & Automation

### AWS SAM (Serverless Application Model)
- Simplified framework for **serverless app deployments** (Lambda, API Gateway, DynamoDB).  
- Compiles to **CloudFormation templates**.  
- Commands:  
  - `sam build` → package code.  
  - `sam deploy` → deploy stack.  
  - `sam local invoke -e event.json` → run/test locally.  
- Supports **test events**, **stages**, and **parameters**.  

✅ Gotchas: SAM = CloudFormation under the hood. `$LATEST` not safe for prod → use **aliases**.  
💡 Memory Hook: “SAM = Serverless CFN.”  

---

### AWS CDK (Cloud Development Kit)
- Define infra using languages (TS, Python, Java, C#).  
- Produces **CloudFormation templates**.  
- Allows reusable **constructs** for patterns.  

✅ Gotchas: CDK doesn’t deploy directly → always through CloudFormation.  
💡 Memory Hook: “CDK = Cloud Defined in Code.”  

---

### AWS CloudFormation
- Core IaC engine for AWS.  
- Creates, updates, deletes stacks.  
- Supports **Change Sets** (preview before deploying).  
- Basis for SAM & CDK.  

✅ Gotchas: Failed stacks roll back by default.  

---

### AWS CLI
- Scripted deployments.  
- Examples:  
  - `aws lambda update-function-code`  
  - `aws apigateway create-deployment`  
- Useful for test events + automation.  

✅ Gotchas: Great for scripts, but not full IaC.  
💡 Memory Hook: “CLI = Quick deploy/debug.”  

---

### Amazon S3
- Stores **deployment artifacts** (Lambda zips, CloudFormation templates).  
- Used by **CodePipeline** as artifact store.  
- Supports versioning for rollback.  

✅ Gotchas: All SAM/CFN artifacts must be uploaded to S3.  
💡 Memory Hook: “S3 = Deployment bucket.”  

---

## 2️⃣ CI/CD Orchestration

### AWS CodePipeline
- Automates CI/CD → build → test → deploy.  
- Integrates with CodeCommit/GitHub, CodeBuild, CodeDeploy, ECS, Lambda.  

✅ Gotchas: Orchestrates only, doesn’t build code.  

---

### AWS CodeBuild
- Fully managed build service.  
- Runs **tests, packaging, Docker builds**.  
- Uses **buildspec.yml**.  

✅ Gotchas: Pay per build minute.  

---

### AWS CodeDeploy
- Deployment strategies:  
  - In-place (update in place).  
  - Blue/green (shift traffic gradually).  
- Works with **EC2, ECS, Lambda**.  

✅ Gotchas: For Lambda, CodeDeploy manages traffic between versions/aliases.  

---

### AWS Amplify
- Deploys full-stack web/mobile apps.  
- Git-based CI/CD → each **branch = environment**.  

✅ Gotchas: Branch mapping is exam-tested.  

---

### AWS Copilot
- CLI tool for ECS deployments.  
- Creates environments, services, and pipelines automatically.  

✅ Gotchas: Copilot is ECS-specific (not EKS).  

---

## 3️⃣ Compute: Serverless

### AWS Lambda
- Runs serverless functions.  
- **Versions**: Immutable snapshots.  
- **Aliases**: Pointers to versions (`dev`, `prod`).  
- **Layers**: Shared libraries.  
- Deployment methods: SAM, CloudFormation, CLI, CI/CD.  

✅ Gotchas:  
- Don’t use `$LATEST` in prod.  
- Layers have size limits (50 MB zipped).  
💡 Memory Hook: “Lambda = Functions with Versions & Aliases.”  

---

## 4️⃣ Containers

### Amazon ECR (Elastic Container Registry)
- Stores Docker images.  
- Supports tagging (`:dev`, `:prod`).  

✅ Gotchas: Must authenticate before push/pull.  
💡 Memory Hook: “ECR = Container pantry.”  

---

### Amazon ECS (Elastic Container Service)
- AWS-native container orchestration.  
- **Clusters, tasks, services** = environments.  
- Supports **blue/green deployments** (via ALB + CodeDeploy).  

✅ Gotchas: ECS simpler, AWS-native → exam loves it.  
💡 Memory Hook: “ECS = Easy Containers.”  

---

### Amazon EKS (Elastic Kubernetes Service)
- Managed Kubernetes.  
- Supports **Helm charts, namespaces**.  
- Integrates with CI/CD pipelines.  

✅ Gotchas: More complex than ECS; exam tests high-level knowledge.  
💡 Memory Hook: “EKS = Kubernetes on AWS.”  

---

## 5️⃣ Networking & Routing

### Elastic Load Balancer (ELB)
- Routes traffic across resources.  
- **Types**:  
  - ALB = HTTP/HTTPS, microservices, containers.  
  - NLB = High performance, TCP/UDP.  
  - CLB = Legacy.  
- Required for ECS/EKS production.  
- Enables **blue/green deployments** with CodeDeploy.  

✅ Gotchas:  
- ALB + Lambda → must return JSON.  
- Enable cross-zone LB for HA.  
💡 Memory Hook: “ELB = Traffic director.”  

---

### Amazon VPC
- Virtual network for AWS.  
- Isolates **dev/test/prod** via subnets.  
- Required for ECS/EKS tasks, private Lambdas.  

✅ Gotchas: Lambda often fails if VPC/security groups not configured.  
💡 Memory Hook: “VPC = Deployment playground.”  

---

### Amazon API Gateway
- Deploy/manage APIs.  
- Uses **stages** (dev, test, prod).  
- Requires redeployment after changes.  
- Supports **stage variables**.  

✅ Gotchas: Clients don’t see changes until redeployment.  
💡 Memory Hook: “API Gateway = Stage manager.”  

---

## 6️⃣ Security

### AWS IAM
- Defines **who can deploy what**.  
- CI/CD pipelines assume **IAM roles**.  
- ECS/Lambda tasks need **execution roles**.  

✅ Gotchas: Many deployment failures = missing assume-role permissions.  
💡 Memory Hook: “IAM = Permissions gatekeeper.”  

---

# ⚡ Master Gotchas & Memory Hooks

- **IaC**: SAM/CDK = CloudFormation-based → automate infra.  
- **CI/CD**:  
  - CodePipeline = orchestrator.  
  - CodeBuild = builds/tests.  
  - CodeDeploy = deployment strategy (blue/green, canary).  
- **Lambda**: Aliases = stable environments. `$LATEST` = ❌. Layers = shared deps.  
- **ECR/ECS/EKS**: Container workflow. ECR stores images → ECS/EKS run them.  
- **ELB**: Required for zero-downtime deployments. Blue/green supported with CodeDeploy.  
- **Amplify**: Branch = environment.  
- **Copilot**: ECS deployment CLI.  
- **API Gateway**: Redeploy after changes. Stages = environments.  
- **IAM**: Permissions cause silent deployment failures.  
- **S3**: Artifact bucket for SAM/CFN/CodePipeline.  
- **VPC**: Needed for private deployments.  

---
