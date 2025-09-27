# **ALLOWING SOME AUTHORIZED CLIENTS TO INVALIDATE CACHE**

## Recommended
- **API Gateway + 0️⃣ = authorized front door** (respects cache rules)  
  - Use **Require Authorization** in cache settings.  
  - Clients send **Cache-Control: max-age=0** to bypass cache.

## Not Recommended
- **STS = backdoor access** (bypasses the API)  
  - Giving clients direct DynamoDB access does **not** invalidate API Gateway cache.  
  - Bypasses API-level control, logging, throttling, and security.
 

# **Lambda Invocation Types**

- **Synchronous (RequestResponse)** ⏳: Caller waits, gets result immediately.  
- **Asynchronous (Event)** 🚀: Caller doesn’t wait, Lambda retries on failure. ---ASYNC IS ALSO CALLED AS **EVENT ** 
- **Destinations**: Handle success/failure of async invocations externally (SNS, SQS, Lambda).

