---
title: "AWS Lambda"
menutitle: "AWS Lambda"
weight: 1
chapter: true
pre: "<b></b>"
---


AWS Lambda is a compute service that allows you to run code without provisioning or managing servers. AWS Lambda only executes code when needed and scales automatically, from a few requests a day to thousands per second. You only pay for the compute time you use; there will be no charge when your code isn't running.

{{% notice tip%}}
The cost model of Serverless applications using AWS Lambda is a major advantage in scenarios of *Disaster Recovery* since you only pay for usage, not for the availability of allocated resources.
{{% /notice%}}

With AWS Lambda, you can run code for any type of backend application or service virtually, all without administration. The following [languages are compatible with AWS Lambda](https://docs.aws.amazon.com/pt_br/lambda/latest/dg/lambda-runtimes.html).

#### Multi-region Lambda Management

Use versioning tools to manage your AWS Lambda functions such as [CodeCommit](https://aws.amazon.com/codecommit/) for example. It is not the focus of this page to address the topic, but it is also possible to store your source code in multiple regions. This blog post [blog post](https://aws.amazon.com/blogs/devops/replicate-aws-codecommit-repository-between-regions-using-aws-fargate/) describes a possible architecture for this.

The strategies of *Disaster Recovery* require their AWS Lambda functions to be installed in more than one region. There is no automatic way to do this, since the regions are completely independent. Below, we describe the possibilities for each approach:

##### Backup & Restore

It consists of maintaining a backup of your AWS Lambda functions so that you can restore them in another region in the event of a disaster. Keep the source code and settings of your functions under the management of a versioning tool and use tags to baseline your code versions. During the restore process, you can install your functions in two ways: [Manually]({{< ref "manual" >}}  "Manually") or through a [CI/CD Pipeline]({{< ref "pipeline" >}} "CI/CD Pipeline").

##### Pilot Light

For this strategy, AWS Lambda functions that are critical to the minimal functioning of the DR region must be functional and up to date. In this way, consider minimally that critical AWS Lambda functions are managed via [CI/CD Pipeline] ({{< ref "pipeline" >}} "CI/CD Pipeline"). At the time of provisioning the production facility in the event of a disaster, provision the environment also through the [CI/CD Pipelines] ({{< ref "pipeline" >}} "CI/CD Pipeline") or, at least, [< ref "manual" > manually] ({{}} "Manually").

##### Warm Standby & Active-Active

In this case, you will have two functional productive environments, but with one of them running at minimum capacity. Maintaining both manually is unfeasible, and in this case the [CI/CD Pipeline] ({{< ref "pipeline" >}}) is the option to use.

---

**References**

- [AWS Lambda](https://docs.aws.amazon.com/pt_br/lambda/latest/dg/welcome.html)
- [Replicate AWS CodeCommit Repositories between Regions using AWS Fargate](https://aws.amazon.com/blogs/devops/replicate-aws-codecommit-repository-between-regions-using-aws-fargate/)
