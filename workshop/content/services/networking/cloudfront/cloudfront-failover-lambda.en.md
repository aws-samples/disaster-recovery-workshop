---
title: "Amazon CloudFront - Failover using Lambda@Edge"
menutitle: "Failover using Lambda@Edge"
chapter: false
pre: "<b></b>"
awsServices: 
 - Amazon CloudFront
tags: 
 - Active-Active
---

You can use Lambda @Edge functions with CloudFront distributions that you have configured with origin groups. To use a Lambda function, specify it in an origin request trigger or source response from an origin group when creating the cache behavior. When using a Lambda @Edge function with a source group, it can be triggered twice for a single viewer request. For example, consider this scenario:

1.  You create a Lambda @Edge function with a source request trigger.

2.  The Lambda function is triggered when CloudFront sends a request to the primary origin (in a cache failure).

3.  The primary source responds with an HTTP status code configured for failover.

4.  The Lambda function is triggered again when CloudFront sends the same request to the secondary origin.

![Origins Groups with Lambda@Edge](/images/cloudfront-origingroups-with-lambda-edge.png)

---
**References**
- [Use origin failover with Lambda@Edge functions](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/high_availability_origin_failover.html#concept_origin_groups.lambda)
