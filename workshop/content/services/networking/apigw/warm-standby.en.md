---
title : "Amazon API Gateway - Warm Standby"
menuTitle: "Warm Standby"
tags:
  - Warm Standby
awsServices:
  - Amazon API Gateway
---

In the case of external apis accessible over the internet, a possible configuration that supports a warm standby scenario for your apis in different regions is as follows as follows in the figure below.

Supported by Route53 routing policies, you can direct traffic from your apis to an infrastructure in a region, and through the health check functionality, the service notices the occurrence of an outage and switches DNS entries to direct traffic to another region where your apis are already defined and deployed.

![API Gateway Multi-region](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2017/11/08/active-active-architecture.png)

***

**References:**

*   [Building a serverless multi region with Amazon API Gateway and AWS Lambda](https://aws.amazon.com/blogs/compute/building-a-multi-region-serverless-application-with-amazon-api-gateway-and-aws-lambda/)
