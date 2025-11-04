🌩️ Terraform Revision — AWS Resume Challenge Project

Context:
I have an old AWS Terraform project open in VS Code that I built a year ago. I’ve forgotten most of Terraform, and I have an interview today.

🎯 Goals

Analyze the Terraform project from the workspace

Explain what it’s doing — which resources it creates, and how providers, variables, outputs, and modules are structured

Teach Terraform concepts using this code as context

Identify key Terraform topics to revise for the interview

Create a cheat sheet for quick revision

🏗️ Project Overview: AWS Resume Challenge with Visitor Counter

A static website hosting solution with a visitor counter feature — a classic cloud resume challenge demonstrating full-stack AWS + Terraform skills.

Architecture Summary
Layer	Components	Description
Frontend	S3, CloudFront, Route53	Static website hosting with CDN and custom domain
Backend	Lambda, DynamoDB	Serverless API for visitor counter
Infra Mgmt	Terraform	IaC with remote state in S3
📚 Terraform Concepts Explained (Using My Code)
1. Providers
provider "aws" {
  region = "us-east-1"
}


Providers are plugins Terraform uses to manage resources.
👉 Here: AWS provider to manage AWS resources in us-east-1.

2. Backend & State Management
terraform {
  backend "s3" {
    bucket  = "nehaw-terraform-backend"
    key     = "aws-resume-challange/terraform.tfstate"
    region  = "us-east-1"
    encrypt = true
  }
}


Defines where Terraform stores its state file.
👉 Remote state in S3 — allows collaboration and CI/CD.

3. Variables
variable "s3_bucket_name" {
  type = string
}


Variables make configs reusable.
👉 Used for names of buckets, Lambda, domain, etc.

4. Resources
resource "aws_lambda_function" "visitorCounter" {
  filename      = data.archive_file.zip_the_mjs_code.output_path
  function_name = var.lambda_function_name
  role          = aws_iam_role.iam_for_lambda.arn
  handler       = "index.handler"
  runtime       = "nodejs20.x"
}


Resources are actual infrastructure pieces.
👉 Creates Lambda, S3, DynamoDB, CloudFront, etc.

5. Data Sources
data "archive_file" "zip_the_mjs_code" {
  type        = "zip"
  source_file = "${path.module}/lambda/index.mjs"
  output_path = "${path.module}/lambda/index.zip"
}


Data sources read info or generate artifacts.
👉 Zips Lambda code before deployment.

6. Locals
locals {
  source_directory = "${path.module}/../CherryBlossom"
  files            = fileset(local.source_directory, "**")
}


Locals hold computed values.
👉 Used for S3 uploads and file paths.

🧩 Infrastructure Breakdown
Frontend (front-end.tf)

S3 Bucket: Hosts static site

S3 Objects: Uploads site files via for_each

CloudFront Distribution: CDN with SSL and georestrictions

Route53 Record: DNS alias → CloudFront

Origin Access Control: Secures S3 via CloudFront only

Backend (back-end.tf)

Lambda Function: Node.js visitor counter

IAM Role/Policy: DynamoDB access

Lambda Function URL: Public endpoint with CORS

DynamoDB Table: Stores visitor counts

Config File: Writes config.js with Lambda URL for frontend

💡 Key Terraform Topics for Interview
1. State Management

Remote backend (S3)

Encryption

Locking (DynamoDB)

Drift detection

2. Dependencies

Implicit (resource references)

Explicit (depends_on)

Interpolation syntax

3. Advanced Features

for_each loops

locals

lifecycle blocks

Data sources vs resources

4. AWS Patterns

IAM least privilege

CloudFront OAC

Lambda + DynamoDB integration

CORS setup

5. Security

Block S3 public access

Encrypted backend

SSL certificates via CloudFront

IAM policy scoping

📋 Terraform Interview Cheat Sheet
Commands
terraform init
terraform plan
terraform apply
terraform destroy
terraform fmt
terraform validate
terraform state list

Concepts
Concept	Key Point
State	Stored in remote S3 backend
IaC Benefits	Version control, repeatability, drift detection
Resource Mgmt	Implicit deps, for_each loops, lifecycle ignore_changes
Security	Least privilege IAM, encryption, OAC
Code Patterns

Dynamic S3 File Uploads

resource "aws_s3_object" "files" {
  for_each = tomap({ for file in local.files : file => file })
  bucket   = aws_s3_bucket.front-end-s3-bucket.bucket
  key      = each.key
  source   = "${local.source_directory}/${each.value}"
}


Resource References

function_name = aws_lambda_function.visitorCounter.function_name
role           = aws_iam_role.iam_for_lambda.arn


Environment Variables

s3_bucket_name = "nehaw-cherry-blossom-dc123-s3"

🗣️ Common Interview Q&A

Q: How do you manage Terraform state?
A: I use an S3 remote backend with encryption and state locking via DynamoDB.

Q: How do you handle secrets?
A: Avoid hardcoded values; use AWS Secrets Manager or Parameter Store.

Q: How do you manage environments?
A: Use .tfvars files or workspaces for state isolation.

Q: How do you ensure security?
A: IAM least privilege, OAC for CloudFront, encrypted state, no public buckets.

🚀 Quick Interview Prep Points

✅ Full-stack AWS architecture
✅ IaC with Terraform
✅ Serverless design (Lambda + DynamoDB)
✅ CDN + DNS management
✅ Security best practices
✅ CI/CD readiness

Strong Talking Points

Built serverless visitor counter using Terraform

Secured CloudFront-S3 integration with OAC

Used for_each for dynamic S3 uploads

Managed remote state in S3

Brush Up

Workspaces

State locking

Module creation

Terraform import

You’re ready!
This project proves understanding of Terraform + AWS + IaC — perfect for your interview. 🌟
