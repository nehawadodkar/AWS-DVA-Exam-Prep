# ⚡ AWS Elastic Beanstalk – DVA Exam Notes

## 1️⃣ Overview
- **Elastic Beanstalk (EBS)** = PaaS to deploy and manage applications easily.  
- Supports: **Java, Node.js, Python, Go, .NET, Docker, Ruby, PHP**.  
- Handles **provisioning, load balancing, scaling, monitoring** automatically.  

## 2️⃣ Application Deployment
- Deploy via **ZIP / WAR / Docker image**.  
- **Single-container Docker** → use standard Docker platform.  
- **Multi-container Docker** → requires `Dockerrun.aws.json` at root of source bundle.  
- `.ebextensions` → YAML configs for environment customizations (packages, files, commands, environment variables).  

✅ Gotcha: For multi-container Docker, **`Dockerrun.aws.json` must be at root**, not inside `.ebextensions`.  

## 3️⃣ Environment Types
- **Web server environment** → Handles HTTP requests (with load balancer).  
- **Worker environment** → Processes background jobs from SQS.  

## 4️⃣ Configuration & Scaling
- **Elastic Load Balancer (ELB)** supported automatically.  
- **Auto Scaling** → Configurable via EBS console, CLI, or `.ebextensions`.  
- **Environment tiers** → Single instance or load-balanced.  

## 5️⃣ Deployment Options
- **All at once** → Fast, but causes downtime.  
- **Rolling** → Deploy in batches, minimal downtime.  
- **Rolling with additional batch** → New instances added for zero downtime.  
- **Immutable** → Launch new environment → safest, zero downtime.  

## 6️⃣ Updates & Customization
- Use **`eb config`** or **console** to update environment settings.  
- Use **`.ebextensions`** to automate config changes on deployment.  
- Can update **environment variables, software packages, container definitions**.  

## 7️⃣ Logs & Monitoring
- CloudWatch metrics integrated.  
- Can request **logs** from EC2 instances or Docker containers.  
- Health monitoring available via console/CLI.  

## 8️⃣ Key Exam Gotchas
- Multi-container Docker → **Dockerrun.aws.json at root** (not in `.ebextensions`).  
- `.ebextensions` is for **custom configs**, not container definitions.  
- `eb config` → updates environment settings, does not define containers.  
- ECS console → Not used when deploying through Elastic Beanstalk.  
- Elastic Beanstalk automatically handles **load balancing, scaling, and monitoring**.

## Elastic Beanstalk .ebextensions Use Case

- **Purpose:** Automate environment configuration during deployment.  
- **Example Use Case:**  
  - Install additional software or packages on EC2 instances.  
  - Create custom directories or config files.  
  - Set environment variables or run commands at deployment.  
