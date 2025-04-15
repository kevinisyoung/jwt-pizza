# Vercel vs. AWS: Infrastructure Comparison for Full-Stack Web Apps

## Introduction

I explored the AWS services that Vercel uses under the hood and compared using Vercel vs manually setting up equivalent services directly on AWS. It's aimed to give me a better idea of what Vercel has going on to make its deployments so reliable.

---

## Vercel’s Under-the-Hood AWS Services

| **Vercel Capability** | **AWS Service Used by Vercel** |
|-----------------------|---------------------------------|
| Static File Storage   | Amazon S3 |
| Build Infrastructure  | AWS EC2 (Fargate), SQS |
| Serverless Functions  | AWS Lambda |
| Edge Functions        | AWS Lambda@Edge or similar |
| CDN & Global Routing  | AWS Global Accelerator, CloudFront, AWS Global Network |
| DNS Management        | AWS Route 53 |
| SSL Certificates      | AWS Certificate Manager (ACM) |

---

## Vercel Deployment & Request Lifecycle

1. **Build Phase**
   - Source code uploaded to S3
   - Job queued in SQS
   - Build containers spin up in AWS Fargate to build the app
   - Output files/functions stored in S3

2. **Global Deployment**
   - DNS points to anycast IP via Global Accelerator
   - Assets/functions distributed across edge regions (S3 + Lambda)

3. **Handling Requests**
   - Request routed to nearest edge region
   - Reverse proxy determines request target:
     - Edge Function (for Middleware)
     - Static File (from S3)
     - Serverless Function (via Lambda)
   - Responses cached at edge and returned to user

---

## DIY AWS Setup Equivalent

| **Vercel Functionality** | **Manual AWS Setup** |
|--------------------------|------------------------|
| Static hosting           | S3 with CloudFront |
| CDN                      | CloudFront (w/ Lambda@Edge optional) |
| Serverless Functions     | Lambda + API Gateway |
| Edge Functions           | Lambda@Edge / CloudFront Functions |
| Builds & CI/CD           | GitHub actions |
| DNS                      | Route 53 |
| SSL                      | ACM, attached to CloudFront or ALB |
| Caching & Invalidation   | CloudFront config and CLI/API tools |

---

## Pros and Cons

### Vercel
- **Pros**
  - Zero-config deployments
  - Great developer experience
  - Automatic edge delivery
  - Built-in CI/CD, previews, HTTPS
  - Ideal for Next.js and Jamstack apps

- **Cons**
  - Limited to specific architectures
  - Potentially higher costs at scale
  - Black-box infrastructure, less AWS learning

### AWS Manual Setup
- **Pros**
  - Full flexibility and control
  - Potential cost savings at scale
  - Learn cloud services deeply
  - Can integrate broader AWS services (e.g., RDS, DynamoDB, SQS, etc.)

- **Cons**
  - Steeper learning curve
  - More setup & maintenance
  - No built-in frontend framework detection or optimizations

---

## When to Choose What

- **Choose Vercel if:**
  - You want fast, easy deployments with minimal cloud expertise
  - Your app fits a serverless + static architecture
  - You want a good developer experience with built-in CI/CD

- **Choose AWS (DIY) if:**
  - You want to learn how cloud services work
  - You need advanced backend logic or long-running tasks
  - You want complete control over cost, scaling, and regions

---

## Conclusion

Vercel offers an amazing developer experience by abstracting away the complexity of AWS services while leveraging AWS's global infrastructure (S3, Lambda, CloudFront, etc.). If you need a quick and scalable way to launch full-stack apps without touching infrastructure, Vercel is great. However, for full customization, learning AWS, and deep control, setting up your stack on AWS is a powerful and educational path.

---

## Sources

- Vercel Engineering Docs
- AWS Documentation
- Case studies: Graphite.dev, community comparisons
- Medium articles and community discussions on Dev.to, Reddit, and GitHub

---

