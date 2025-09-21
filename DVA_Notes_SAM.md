# ⚡ AWS Serverless Application Model (AWS SAM) – Exam Notes

## 🚀 What is SAM?
- **Framework + CLI** for building, testing, and deploying **serverless applications**.  
- Focused on: **Lambda, API Gateway, DynamoDB, S3, SNS/SQS, CloudFront**.  
- **Extension of CloudFormation** → SAM syntax is just shorthand that CloudFormation expands.

---

## 🗂️ Core Concepts

### 🔹 Transform (✅ Exam Critical)
- Required in any SAM template:
  ```yaml
  Transform: AWS::Serverless-2016-10-31
  ```
- Tells CloudFormation: “Process this template using SAM macros.”  
- Without this, SAM syntax (`AWS::Serverless::*`) won’t work.

---

### 🔹 Resources
- **AWS::Serverless::Function** → Lambda.  
- **AWS::Serverless::Api** → API Gateway.  
- **AWS::Serverless::SimpleTable** → DynamoDB.  
- **AWS::Serverless::LayerVersion** → Lambda Layer.  

✅ SAM expands these into full CloudFormation resources.

---

### 🔹 Globals
- Section to define **default configs** (applied to all resources).  
- Example:
  ```yaml
  Globals:
    Function:
      Timeout: 10
      MemorySize: 256
  ```

---

### 🔹 Deployment Flow
1. Write `template.yaml` (SAM template).  
2. `sam build` → prepares code + dependencies.  
3. `sam package` → uploads code to S3, rewrites template.  
4. `sam deploy` → deploys via CloudFormation.  
5. Optional: integrate with **CodeDeploy** for canary/linear rollouts.

---

## 🛠️ SAM CLI (must-know commands)
- `sam init` → create new project.  
- `sam build` → build source code + dependencies.  
- `sam local invoke` → run Lambda locally.  
- `sam local start-api` → run API Gateway locally.  
- `sam package` → package + upload to S3.  
- `sam deploy --guided` → interactive deploy.

---

## 🧩 Example Template
```yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Description: Simple SAM App with Lambda + API Gateway + DynamoDB

Globals:
  Function:
    Timeout: 5
    MemorySize: 128

Resources:
  MyTable:
    Type: AWS::Serverless::SimpleTable
    Properties:
      PrimaryKey:
        Name: id
        Type: String

  MyFunction:
    Type: AWS::Serverless::Function
    Properties:
      Handler: app.lambda_handler
      Runtime: python3.9
      Events:
        ApiEvent:
          Type: Api
          Properties:
            Path: /items
            Method: get
      Environment:
        Variables:
          TABLE_NAME: !Ref MyTable
```

---

## ✅ Exam Gotchas
- **Transform section is mandatory** → `AWS::Serverless-2016-10-31`.  
- SAM = shorthand → always compiles down to CloudFormation.  
- SAM supports **safe deployments** via **CodeDeploy** (canary, linear, all-at-once).  
- **Globals** let you define defaults (common exam trick).  
- **Local testing** requires Docker.  
- **SAM vs CDK**:  
  - SAM = YAML, serverless focus.  
  - CDK = general-purpose IaC in real programming languages.  

---

## ⚡ Memory Hooks
- **SAM = Shorter CloudFormation for Serverless**.  
- **Transform = Unlocks SAM syntax**.  
- **Globals = Defaults for all functions/APIs**.  
- **CLI = Build, Test, Deploy locally + to AWS**.  
- **SAM + CodeDeploy = Safe rollouts**.  
