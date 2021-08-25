---
title: "Amazon CloudFront - Origin Group"
menutitle: "CloudFront - Origin Group"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Warm Standby]
type: Lab
awsServices:
 - Amazon CloudFront
---

### Objective

This exercise will show you the steps for using the Amazon CloudFront Origin Group engine, using Amazon S3 buckets as an origin. During this lab it will be created two S3 buckets and when the Cloudfront Distribution receive a error code from S3 on Primary Region it will try the same request on Secondy Region automacically.  

If you want more information [click here]({{< ref "/services/networking/cloudfront" >}} "Amazon CloudFront").

![Cloudfront Failover](/images/cloudfront-failover.png)

**By the end of this exercise, you will be able to:**

- Create resilient CloudFront distributions with two origins, one primary and a secondary for failover.

**Estimated Duration:** 30 minutes

**Approximate Cost**: 1 USD

### Execution
{{< tabs name="labs_types" >}} 
{{< tab name="AWS CloudShell" include="cloudshell" />}}  
{{< /tabs >}}

### Conclusion

With this exercise, it was possible to verify how Amazon CloudFront distributions can be resilient by using two or more origins with the Origin Group feature in a strategy of the type. *Active-Passive*.

**Source:** [Amazon CloudFront - Developer Guide](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/high_availability_origin_failover.html#concept_origin_groups.creating)

