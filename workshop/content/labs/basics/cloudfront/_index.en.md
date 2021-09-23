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

This exercise will show you the steps for using the Amazon CloudFront Origin Group mechanism, using Amazon S3 buckets as the origins. During this lab it will be created two S3 buckets and when the Amazon Cloudfront distribution receive a error code from Amazon S3 on Primary Region, it will trigger the same request on Secondary Region automatically.  

If you want more information [click here]({{< ref "/services/networking/cloudfront" >}} "Amazon CloudFront").

**By the end of this exercise, you will be able to:**

- Create resilient Amazon CloudFront distributions with two origins, the primary to deliver content and the secondary for failover.

**Estimated Duration:** 30 minutes

**Approximate Cost**: 1 USD

### Solution Overview
![Cloudfront Failover](/images/cloudfront-origin-failover.png)


### Execution
{{< tabs name="labs_types" >}} 
{{< tab name="AWS CloudShell" include="cloudshell" />}}  
{{< /tabs >}}

### Conclusion

With this exercise, it was possible to verify how Amazon CloudFront distributions can be resilient by using two or more origins with the Origin Group feature in a strategy of the type *Warm-Standby*.

**References:** 
- [Amazon CloudFront - Developer Guide](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/high_availability_origin_failover.html#concept_origin_groups.creating)

