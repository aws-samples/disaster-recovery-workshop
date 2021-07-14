---
title: "AWS Security Token Service"
menutitle: "AWS STS"
weight: 1
chapter: true
pre: "<b></b>"
---

## Regional Endpoints

By default, AWS Security Token Service (AWS STS) is available as a global service, and all AWS STS requests go to a single endpoint at https://sts.amazonaws.com. AWS recommends using regional AWS STS endpoints instead of the global endpoint to reduce latency, incorporate redundancy, and increase session token validity.

Reduce Latency — By making your AWS STS calls to an endpoint closer to your services and applications, you can access AWS STS services with lower latency and shorter response times.

Embed redundancy — You can add code to the application that changes your AWS STS API calls to a different region. This ensures that if the first region stops responding, the application will continue to work. This redundancy is not automatic, you should create the functionality in your code.

Increase Session Token Validity — AWS STS Regional Endpoint Session Tokens are valid across AWS Regions. STS Global Endpoint Session Tokens are valid only in AWS Regions that are enabled by default. If you want to enable a new region for your account, you can use session tokens from STS regional endpoints. If you choose to use the global endpoint, you must change the compatibility of STS session token regions for the global endpoint. This ensures that the tokens are valid in all AWS regions.

{{% notice tip%}} <i class="fas fa-lightbulb"></i>
See the table for [STS regional endpoints](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html#id_credentials_region-endpoints) and make sure your application is using at least two regional endpoints.
{{% /notice%}}

***

## VPC Endpoints for AWS STS

If you use Amazon Virtual Private Cloud (Amazon VPC) to host your AWS resources, you can establish a private connection between your VPC and AWS STS. You can use this connection to enable AWS STS to communicate with your resources in the VPC without going through the public Internet.

Amazon VPC is an AWS service that you can use to run AWS resources on a virtual network that you define. With VPC, you have control over network settings, such as the range of IP addresses, subnets, route tables, and network gateways. To connect your VPC to AWS STS, you define an interface VPC endpoint for AWS STS. The endpoint provides reliable and scalable connectivity to AWS STS without the need for an Internet gateway, a network address conversion (NAT) instance, or a VPN connection.

***

**References:**

*   [Using AWS Security Token Service in Your .NET Application](https://www.youtube.com/watch?v=9e2DdGT5mZg)
