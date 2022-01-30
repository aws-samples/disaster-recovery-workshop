---
title : "Amazon API Gateway - Active-Active"
menuTitle : "Active-Active"
tags:
  - Active-Active
awsServices:
  - Amazon API Gateway
---

In the case of external apis accessible over the internet, a possible configuration that supports an active-active scenario for your apis in different regions may be as shown below.

Supported by Route 53 routing policies, you can direct traffic from your apis to more than one infrastructure in different regions, and through weight-routing policies, the service allows you to balance requests according to the infrastructure capacity of each region.

![API Gateway Multi-region](/images/route-53-healthcheck.png)

---
**References:**
- [Building a serverless multi region with Amazon API Gateway and AWS Lambda](https://aws.amazon.com/blogs/compute/building-a-multi-region-serverless-application-with-amazon-api-gateway-and-aws-lambda/)
