---
title: "Amazon ECR - Image Registry Replication"
menutitle: "ECR - Image Registry Replication"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Active-Active]
type: Lab
awsServices:
 - Amazon ECR
---

### Objective

This exercise will show you the steps to enable record replication in the Amazon Elastic Container Registry. In a multi-region scenario, stored container images must be available in a secondary region before the application can be configured during the disaster.

**By the end of this exercise, you will be able to:**

*   Set Up Replication to Another Region of an Amazon ECR Image Record
*   List images and verify that replication is working according to the applied configuration.

**Estimated Duration:** 15 minutes

**Approximate Cost**: 1 USD

### Execution
{{< tabs name="labs_types" >}}  
{{< tab name="AWS Cloud9" include="cloud9" />}}
{{< /tabs >}}

### Conclusion

With this exercise, it was possible to understand how to set up an Amazon Elastic Container Registry image repository, push an image to the repository, configure replication with a second region, update the image, and verify that image replication was successfully performed for the second region .

**Source:** [Cross region replication in Amazon ECR has landed](https://aws.amazon.com/pt/blogs/containers/cross-region-replication-in-amazon-ecr-has-landed/)
