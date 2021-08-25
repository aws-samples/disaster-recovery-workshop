---
title: "AWS EKS - DR Environment Using Kubefed"
menutitle: "EKS - Kubefed"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Warm Standby]
type: Lab
awsServices:
 - Amazon EKS
---


{{% notice warning%}}
**Note:** On the date of publication of this mini lab, the current version of KubeFed is not in a release state and is recommended for testing purposes only. Check the [KubeFed status](https://github.com/kubernetes-sigs/kubefed) if you plan to use it.
{{% /notice%}}

{{% notice tip%}}
**Note:** There are other methods for using EKS in a multi-region scenario. For more details visit:
[Using GitOps and AWS EKS to Manage HA & DR](https://weaveworks-gitops.awsworkshop.io/25_workshop_2_ha-dr.html)

{{% /notice%}}

### Objective

This exercise will show you the steps to set up and use [KubeFed](https://github.com/kubernetes-sigs/kubefed) that allows you to coordinate the configuration of multiple Kubernetes clusters from a single set of APIs in a cluster and can be used in a multi-region scenario.

**By the end of this exercise, you will be able to:**

- Configure KubeFed to coordinate the configuration of multiple Kubernetes clusters
- Deploy an application that will be created on two Kubernetes clusters in target regions
- Use Amazon Route53 to configure DNS resolution with healthcheck for both regions

**Estimated Duration:** 1 hour

**Approximate Cost**: 10 USD

### Solution Overview

![KubeFed - Federated clusters](/images/kubefed-arch.png)

### Execution

{{% notice info%}}
If you haven't already created your environment on AWS Cloud9, start by creating your workspace. [Click here]({{% relref workspace %}}).
{{% /notice%}}

{{% notice note%}}
For this exercise, use **Virginia (us-east-1)** as the main region and the **Ohio (us-east-2)** as the secondary region.
{{% /notice%}}

{{< tabs name="labs_types" >}}  
{{< tab name="AWS Cloud9" include="cloud9" />}}
{{< /tabs >}}

### Conclusion

With this exercise, it was possible to understand how you can federate multiple clusters into a single cluster to use a single Deployment and Service manifest to replicate a POD across multiple regions, in this case in the region **us-east-1** and **us-east-2**.
