---
title: "AWS Secrets Manager - Secrets Replication"
menutitle: "Secrets Manager - Replication"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Active-Active]
type: Lab
awsServices:
 - AWS Secrets Manager
---

### Introduction

AWS Secrets Manager helps you protect the secrets needed to access IT applications, services, and resources. The service allows you to easily switch, manage, and retrieve database credentials, API keys, and other secrets throughout its lifecycle. Users and applications regain secrets with a call to Secrets Manager APIs, which eliminates the need to encode sensitive information in plain text.
It is essential that we be prepared for any failures, given the importance of a key stored in AWS Secrets Manager, it is crucial to have a replica of it across-region. This is what the following exercise will teach, replication of the secret key in more than one region.

### Solution Overview

![Secrets Manager Architecture](/images/secretmanager-architecture.png)

### Objective

This exercise will show you the step-by-step to replicate your secret key (Secret) in AWS Secrets Manager from one source region to another within AWS.

**Required knowledge:**

- Basic use of the AWS Console

**By the end of this exercise, you will be able to:**

- Replicate AWS Secrets Manager Secrets Manager Cross-AWS Regions

**Estimated Duration:** 20 minutes

**Approximate Cost**: 1 USD

### Overview

In this exercise, the following steps will be performed:

1.  Replicate the Secret to Another Region
2.  Find the Replicated Secret

{{% notice note%}}
**NOTE:** In this exercise, the key will be replicated from the primary region. **Virginia** for the secondary region **Ohio**, however, this same procedure can be performed in other regions.
{{% /notice%}}

### Execution
{{< tabs name="labs_types" >}} 
{{< tab name="AWS Console" include="console" />}} 
{{< /tabs >}}

### Conclusion

With this exercise you learned how to replicate secret keys (Secrets) to another region using the AWS Secrets Manager service. If any incidents occur, you can use the secret keys to access your resources in another AWS region.

**Source:** [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/)
