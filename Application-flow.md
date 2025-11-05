User
  │
  ▼
Route 53 (DNS)
  │
  ▼
CloudFront (CDN / Caching)
  │
  ▼
Application Load Balancer (ALB)
  │
  ▼
EC2 / Auto Scaling Group (App Servers)  ← Layer 4
  │
  ▼  (Private network + Security Groups)
RDS / Database (Relational DB)         ← Layer 5
  │
  ▲
Response flows back through the same path:
EC2 → ALB → CloudFront → Route 53 → User
