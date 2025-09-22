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




# AWS CloudWatch Cheat Sheet (DVA Exam)

## Overview
- **Purpose**: Monitoring, observability, and operational insights for AWS resources & applications.
- **Can monitor**: Metrics, logs, events, and traces (via X-Ray).
- **Use cases**:
  - Track resource utilization (CPU, memory, disk, network)
  - Detect anomalies & trigger alarms
  - Debug application issues

---

## 1. CloudWatch Metrics
- **Definition**: Numerical data points that represent the state of a resource over time.
- **Types**:
  - **AWS service metrics**: Built-in (EC2 CPUUtilization, RDS FreeStorageSpace, API Gateway 4XXError/5XXError, Lambda Duration)
  - **Custom metrics**: Push your own app metrics via SDK or CLI
- **Units**: Count, Seconds, Bytes, Percent, etc.
- **Statistic options**:
  - `Average`, `Sum`, `Minimum`, `Maximum`, `SampleCount`

### Key Metric Examples
| Service          | Metric Name             | Description |
|-----------------|------------------------|------------|
| EC2              | CPUUtilization         | Percent CPU used |
| Lambda           | Duration               | Execution time in ms |
| API Gateway      | 4XXError / 5XXError    | Client/server errors |
| RDS              | FreeStorageSpace       | Remaining storage |

---

## 2. CloudWatch Logs
- **Purpose**: Store, monitor, and analyze log files from AWS resources & apps.
- **Features**:
  - Log groups → collections of log streams
  - Log streams → sequence of log events
  - Can search, filter, and trigger alarms on log patterns
- **Enabling Logs**:
  - Must enable for services like API Gateway, Lambda
- **Useful for**:
  - Debugging errors
  - Tracking request/response payloads
  - Anomaly detection

---

## 3. CloudWatch Alarms
- **Definition**: Triggered when a metric crosses a threshold.
- **Can trigger**:
  - SNS notifications
  - Auto Scaling actions
  - EC2 actions (stop, terminate, reboot)
- **Alarm states**:
  - `OK` → metric is normal
  - `ALARM` → metric breached threshold
  - `INSUFFICIENT_DATA` → not enough data

---

## 4. CloudWatch Events / EventBridge
- **Purpose**: Detect changes in AWS resources and trigger actions.
- **Examples**:
  - EC2 state changes → trigger Lambda
  - Scheduled cron-like events → run periodic tasks
- **Integration**: Lambda, Step Functions, SNS, SQS

---

## 5. CloudWatch Dashboards
- **Purpose**: Visualize metrics & logs in a single pane.
- **Widgets**:
  - Line, stacked area, number, text, gauge
- **Supports**:
  - Metrics from multiple AWS services
  - Custom metrics
  - Multiple regions

---

## 6. CloudWatch and X-Ray
- CloudWatch integrates with **X-Ray** for tracing.
- Allows correlation between logs, metrics, and request traces.
- Useful to identify bottlenecks in distributed applications.

---

## 7. Common Exam Scenarios
- **Monitor EC2 CPU usage** → CloudWatch Metrics + Alarm
- **Debug Lambda errors** → CloudWatch Logs
- **Detect API Gateway high 5XX errors** → CloudWatch Metric + Alarm
- **Track periodic events** → CloudWatch Event / EventBridge
- **Visualize multiple metrics in one place** → CloudWatch Dashboard
- **Trace request latency across services** → CloudWatch + X-Ray

---

