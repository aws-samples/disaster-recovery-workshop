---
title: "Amazon Route 53 - Failover Policy in Private Hosted Zone"
menutitle: "Route 53 - Failover in Private DNS"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Warm-Standby]
type: Lab
awsServices:
 - Amazon Route53
---

### Objective

This exercise will show you the steps for you to quickly configure DNS-based failover functionality for existing endpoints in different regions.

**Required knowledge:**

*   Basic Use of the AWS CLI
*   Load Balancers Getting Started

**By the end of this exercise, you will be able to:**

*   Configure a failover policy on Amazon Route 53 to enable DNS entry to switch for resources in different regions.
*   Architect a failover mechanism for private resources in your VPC.

**Estimated Duration:** 40 minutes

**Approximate Cost**: 3 USD

### Solution Overview

![Route 53 - Architecture](/images/route53-lab-architecture.png)

### Execution
{{< tabs name="labs_types" >}}  
{{< tab name="AWS CloudShell" include="cloudshell" />}}  
{{< /tabs >}}

### Conclusion

With this exercise, it was possible to understand how to configure DNS entries on Route 53 to do dynamic routing between resources in different regions.
For more complex contexts involving on-premises and multi-account environment, the following reference presents a sample architecture that can be applied.

- [Multi region DNS support](https://aws.amazon.com/blogs/architecture/using-route-53-private-hosted-zones-for-cross-account-multi-region-architectures/)

- [Route53 healthchecks for Private Hosted Zone](https://aws.amazon.com/blogs/networking-and-content-delivery/performing-route-53-health-checks-on-private-resources-in-a-vpc-with-aws-lambda-and-amazon-cloudwatch/)
