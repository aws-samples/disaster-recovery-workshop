---
title: "Amazon API Gateway + Route53"
menutitle: "Amazon API Gateway + Route53"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Active-Passive]
type: Lab
awsServices:
 - Amazon API Gateway
---

### Objective

This exercise will show you the steps for you to quickly set up your apis in more than one AWS Region and allow them to behave in an active-passive way by receiving requests transparently from your customers.

**Required knowledge:**

*   Basic Use of the AWS CLI
*   Load Balancers Getting Started

**By the end of this exercise, you will be able to:**

*   Set up an api in two or more regions
*   Set up a DNS engine to dynamically routing your apis transparently to your customers

**Estimated Duration:** 40 minutes

**Approximate Cost**: 3 USD

![Arquitetura API Gateway](/images/apigw-lab-architecture.png)

### Execution

{{< tabs name="labs_types" >}}  
{{< tab name="AWS CloudShell" include="cloudshell" />}}  
{{< /tabs >}}

### Conclusion

With this exercise, we were able to understand how to set up a private api in Amazon API Gateway, deploy the API to more than one region, and create a routing policy for your DNS records associated with your API endpoints.

[Alternative to Custom Domain Name](https://georgemao.medium.com/enabling-private-apis-with-custom-domain-names-aws-api-gateway-df1b62b0ba7c)
