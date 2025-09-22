# AWS X-Ray Cheat Sheet (DVA Exam)

## Overview
- **Purpose**: Distributed tracing system to analyze & debug production/distributed apps.
- **Helps with**: Latency analysis, service maps, error tracing, debugging performance bottlenecks.

## Key Concepts
- **Segment**  
  - A single traced request (e.g., one HTTP request to your app).  
- **Subsegment**  
  - A unit of work within a segment (e.g., DB query, HTTP call, AWS SDK call).  
- **Service Map**  
  - Visual map of services/app interactions built from segments.  

## Annotations vs Metadata
- **Annotations**
  - Key–value pairs.  
  - Indexed by X-Ray → **searchable via filter expressions**.  
  - Example: `userId=123`, `operation="INSERT"`.  

- **Metadata**
  - Key–value pairs.  
  - **Not searchable**, stored only for context/debugging.  
  - Example: raw SQL query string, payload body.  

👉 Exam Tip: *Use **Annotations** for searchable fields, **Metadata** for extra details.*

## How to Enable X-Ray
1. **Elastic Beanstalk**  
   - Add `.ebextensions/debugging.config`:  
     ```yaml
     option_settings:
       aws:elasticbeanstalk:xray:
         XRayEnabled: true
     ```
   - Starts the X-Ray daemon/agent.

2. **Lambda**  
   - Enable X-Ray in function config.  
   - Automatically traces requests.  

3. **EC2 / ECS / On-prem**  
   - Install & run X-Ray daemon.  
   - Instrument code with SDK.  

## SDK Instrumentation
- Required to capture:
  - **HTTP calls** (internal/external).  
  - **AWS SDK calls**.  
  - **SQL queries** (MySQL, PostgreSQL).  
- Supported languages: Java, Node.js, Python, Go, .NET, Ruby.  

### Example: Node.js
```js
const AWSXRay = require('aws-xray-sdk');
const mysql = AWSXRay.captureMySQL(require('mysql'));
