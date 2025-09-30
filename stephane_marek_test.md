AWS requires approximately 5 weeks of usage data to generate budget forecasts

💡 Memory Tip:
If X-Ray shows nothing at all → it’s setup/permissions.
If X-Ray shows too little → it’s sampling.


You can enable aip caching in api gateway - no coding reqd. can do this for simple caching requirement. For complex one, setup elasticache.


- **IAM Access Analyzer** → Finds **unused access** to enforce **least privilege**.  
- **Trusted Advisor** → Gives **best-practice guidance** on cost, security, and performance - used for giving advice for provisioning
- **Amazon Inspector** → **Automated vulnerability scanner** for workloads.  
- **Security Hub** → **Central dashboard** for security findings and compliance.

💡 **Gotcha:** By default, **IAM users cannot access the Billing console**; you must **activate IAM user access once** for policies to work.

- **NACL (Network ACL)** → **Stateless**: Inbound and outbound rules evaluated separately; must allow responses explicitly.  
- **Security Group** → **Stateful**: Response traffic automatically allowed.

💡 **Gotcha:** In IAM, only **role trust policies** are resource-based; everything else (users, groups, roles) uses identity-based policies.

**AWS Serverless Application Repository (SAR)**: A public repository of **pre-built serverless apps** you can deploy directly or customize in your AWS account.

# AWS CloudFormation Template - Common Sections

- **AWSTemplateFormatVersion**  
  Specifies the version of CloudFormation template format (optional).

- **Description**  
  Human-readable description of the template.

- **Metadata**  
  Provides additional information about the template (e.g., for CloudFormation Designer).

- **Parameters**  
  Defines input values that can be passed to the template during stack creation.

- **Mappings**  
  Defines static key-value mappings, e.g., Region → AMI ID.

- **Conditions**  
  Defines rules to conditionally create resources or outputs.

- **Resources**  
  Required section. Declares the AWS resources to be created (EC2, S3, Lambda, etc.).

- **Outputs**  
  Declares values to be returned after stack creation (e.g., VPC ID, S3 bucket name).

- **Transform**  
  (Optional) Specifies macros, e.g., `AWS::Serverless-2016-10-31` for SAM templates.

# CloudFormation Sections - Memory Trick

**“A Dark Monkey Makes Cool Really Outstanding Things”**  
- **A** = AWSTemplateFormatVersion  
- **D** = Description  
- **M** = Metadata  
- **P** = Parameters  
- **M** = Mappings  
- **C** = Conditions  
- **R** = Resources  
- **O** = Outputs  
- **T** = Transform


Grants users access only to their own folder in the S3 bucket.
- **Exam tip:** Know the concept and common variables, but no need to memorize every possible variable.


💡 **Gotcha:** CloudFront key pairs can **only** be created by the **account root user**, not by IAM users or roles.

💡 **AWS SDK vs AWS CDK**  
- **SDK** → Use libraries (Python, Java, JS, etc.) to **call AWS services at runtime** (e.g., upload S3, invoke Lambda).  
- **CDK** → Write infra in code (Python, TypeScript, etc.) → **synthesized to CloudFormation** → deploys AWS resources.  


**CloudFormation StackSet** → Lets you deploy a single CloudFormation stack **across multiple AWS accounts and Regions** in one go.  


****The visibility timeout value for the queue is in seconds, which defaults to 30 seconds**
**

**Elastic Load Balancer**
Build a highly available system
Separate public traffic from private traffic


💡 **Gotcha:** By defaultAWS requires approximately 5 weeks of usage data to generate budget forecasts

