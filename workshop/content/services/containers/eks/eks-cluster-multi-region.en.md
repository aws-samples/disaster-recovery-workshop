---
title: "Amazon EKS - Cluster Multi-Region"
menutitle: "Cluster Multi-Region"
weight: 2
chapter: false
pre: "<b></b>"
awsServices:
 - Amazon EKS
tags: 
 - Active-Active
---

Amazon EKS is a managed service that makes it easy to run Kubernetes on, without the need to install, operate, and maintain your own control plane or Kubernetes nodes. Kubernetes is an open source system for automating the deployment, scalability, and management of containerized applications.

---

## Multi-Region Cluster

This [blog post](https://aws.amazon.com/blogs/containers/operating-a-multi-regional-stateless-application-using-amazon-eks/) demonstrates how you can maintain an Amazon EKS multi-region cluster and can serve as a basis if you need an active-failover or active-active scenario:

![EKS cross-region](/images/eks-cluster-multi-region.jpg)
