### AWS SAM Resource Types – Quick Revision

- **AWS::Serverless::Function** → Defines a Lambda function + event sources (S3, DynamoDB Streams, Kinesis, etc.).  
- **AWS::Serverless::LayerVersion** → Defines a Lambda layer version for shared libraries/runtimes, keeps old versions safe.  
- **AWS::Serverless::Api** → Defines an API Gateway resource; use when you need full control of API configuration.  


### CDA – Troubleshooting and Optimization

**Question:**  
A reporting application is hosted in AWS Elastic Beanstalk and uses DynamoDB. The app currently **scans the entire table** to return data. The table is expected to grow due to new users and increased reporting requests.  
Which steps should be done to improve performance with **minimal cost**? (Select TWO.)

---

### ✅ Correct Answers
- **Use Query operations instead**  
  - Scans read the *entire* table.  
  - Queries are much faster and cheaper as they use keys/indexes to fetch only matching items.  

- **Reduce page size**  
  - Smaller `Limit` reduces the read capacity consumed per request.  
  - Helps avoid throttling and spreads load more evenly across requests.  

---

### ❌ Incorrect Options
- **Use DynamoDB Accelerator (DAX)**  
  - DAX caches **GetItem/Query** operations.  
  - Does **not** optimize Scans.  
  - Adds **extra cost**, not minimal.  

- **Set the ScanIndexForward parameter**  
  - Only changes the order of results.  
  - Does **not** improve performance.  

- **Increase the Write Capacity Units (WCU)**  
  - The workload is **read-heavy**, not write-heavy.  
  - Increases cost unnecessarily.  

---

### 🔑 Exam Tip
If the question mentions **Scan + minimal cost** → the best picks are:  
**Query operations + reduce page size.**  
Use **DAX** only after switching to Query/Key-based reads and if you need further acceleration.
