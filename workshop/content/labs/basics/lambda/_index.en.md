---
title: "AWS Lambda - Exporting functions"
menutitle: "Lambda - Exporting functions"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Backup-Restore, Pilot Light]
type: Lab
awsServices:
 - AWS Lambda
---

### Objective

This exercise will show you the steps for exporting and importing Lambda functions between regions manually. If you want more information [click here]({{< ref "/services/compute/lambda" >}} "AWS Lambda").

**Required knowledge:**

*   Basic Use of the AWS CLI
*   Javascript language basics

**By the end of this exercise, you will be able to:**

*   Export and Import Lambda Cross-Region Functions

**Estimated Duration:** 30 minutes

**Approximate Cost**: 1 USD

### Execution
{{< tabs name="labs_types" >}} 
{{< tab name="AWS Console" include="console" />}} 
{{< tab name="AWS CloudShell" include="cloudshell" />}}  
{{< /tabs >}}

### Conclusion

With this exercise, it was possible to verify how replication of Lambda functions between AWS regions can be used in DR strategies of type. *Backup & Restore* and *Pilot Light*. For strategies for *Warm Standby* and *Active-Active*, refer to the practical exercise for deploying lambda functions via CI/CD Pipeline.

**Source:** [AWS Lambda - Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-awscli.html)


