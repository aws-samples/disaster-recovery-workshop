---
title: "Amazon API Gateway - Failover for Private API"
menutitle: "API Gateway - Failover for Private APIs"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Warm Standby]
type: Lab
awsServices:
 - Amazon API Gateway
---

### Objective

This exercise shows the steps to set up your private APIs in more than one AWS Region using an active-passive disaster recovery strategy. The idea is to failover the API from the primary region to the secondary region in case of failure.

**Required knowledge:**

* Basic usage of the AWS CLI.
* Basic concepts of load balancers. 
* Basic concepts of Route53 and DNS.
* Basic usage of SSL Certificates.

**By the end of this exercise, you will be able to:**

* Build a basic and private REST API in two regions. We will use us-east-1 (North Virginia) as a primary region and us-west-1 (North California) as a secondary region.
* Set up VPC peering between the VPC on primary and secondary regions.
* Set up a DNS record to dynamically route customer requests transparently to your private APIs

**Estimated Duration:** 40 minutes

**Approximate Cost**: 3 USD

![Architecture API Gateway](/images/apigw-lab-architecture.png)

### Execution

{{< tabs name="labs_types" >}}  
{{< tab name="AWS CloudShell" include="cloudshell" />}}  
{{< /tabs >}}

### Conclusion

With this exercise, we were able to understand how to set up a private API in Amazon API Gateway, deploy the API to two regions, and create a routing policy for your DNS records associated with your API endpoints.

**References:**
- [Alternative to Custom Domain Name](https://georgemao.medium.com/enabling-private-apis-with-custom-domain-names-aws-api-gateway-df1b62b0ba7c)
