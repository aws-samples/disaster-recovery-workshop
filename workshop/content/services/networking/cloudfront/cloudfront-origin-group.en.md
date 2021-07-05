---
title: "Amazon CloudFront - Origin Group"
menutitle: "Origin Group"
chapter: false
pre: "<b></b>"
awsServices: 
 - Amazon CloudFront
tags: 
 - Warm Standby
---


You can also configure CloudFront origin failover for scenarios that require high availability. To start, create a source group with two origins: one primary and another secondary. If the primary origin is unavailable or returns specific HTTP response status codes that indicate a failure, CloudFront automatically switches to the secondary origin.

To configure source failover, you must have a distribution with at least two sources. Then create a source group for the distribution that includes two sources, defining one of them as the primary. Finally, create or update a cache behavior to use the origin group.

CloudFront routes all incoming requests to the primary origin, even when a previous request has failed over to the secondary origin. CloudFront only sends requests to the secondary origin after a request fails to the primary origin.

The following diagram shows how source failover works.

![Cloudfront Failover](/images/cloudfront-failover.png)

{{% notice info%}}
CloudFront fails over to secondary origin only when the HTTP method of the consumer request is **GET, HEAD, or OPTIONS**. CloudFront does not fail over when the consumer sends a different HTTP method (for example POST, PUT, etc.).
{{% /notice%}}

**Expected RTO:** A few seconds depending on the configuration of *timeout* for connection to origin, number of attempts, and response timeout.

---
**References**

- [Control origin timeouts and attempts](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/high_availability_origin_failover.html#controlling-attempts-and-timeouts)
