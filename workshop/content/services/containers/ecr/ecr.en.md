---
title: "Amazon ECR - Cross-Region Replication for Images"
menuTitle: "Cross-Region Replication for Images"
weight: 1
chapter: false
pre: "<b></b>"
awsServices:
 - Amazon ECR
type: documentation
tags: 
 - Active-Active
---


Amazon Elastic Container Registry (Amazon ECR) is an AWS container image registration service tool that is secure, scalable, and reliable. Amazon ECR supports private container image repositories with resource-based permissions using AWS IAM so that specific users or Amazon EC2 can access repositories and images. Programmers can use their preferred CLI to push, pull, and manage Docker images, Open Container Initiative (OCI) images, and OCI compatible artifacts.

The AWS Container Services team maintains a public roadmap on GitHub. It contains information about what teams are working on and allows all AWS customers to provide feedback directly. For more information, see [AWS Container Roadmap](https://github.com/aws/containers-roadmap).

---

## Multiregion Image Replication

Considering that its application follows the recommendations of the article above, among them, to have containers *stateless* and Infrastructure-as-Code. You can design your CI/CD automation so that it stores images from your containers after creating the image in two regions, this [blog post](https://aws.amazon.com/blogs/containers/cross-region-replication-in-amazon-ecr-has-landed/) demonstrates how you can do this as shown in the image below:

![ECR cross-region](/images/ecr-crr-scenario-1.png)

It is important to note that even if you have the image of your container in two regions, it is of paramount importance to ensure that your codebase, especially your deployment scripts and infrastructure as code, are available in the event of a disaster in a region because you may need the deployment specification files. .

You can choose to maintain the specification of your pipeline from Amazon ECR and your Amazon ECS and/or Amazon EKS cluster in infrastructure-as-code and create it when needed, or even already keep them provisioned in the second region. We recommend that you test and document the fail-over process to assess whether it meets your RPO/RTO needs and adapt if necessary.

---

**References**

- [Amazon ECR - Private registry setting examples](https://docs.aws.amazon.com/AmazonECR/latest/userguide/registry-settings-examples.html)
