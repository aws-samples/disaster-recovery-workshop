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


O Amazon EKS é um serviço gerenciado que facilita a execução do Kubernetes na, sem a necessidade de instalar, operar e manter seu próprio plano de controle ou nós do Kubernetes. O Kubernetes é um sistema de código aberto para automatizar a implantação, a escalabilidade e o gerenciamento de aplicativos em contêineres.

---
## Cluster Multi-Region

Este [blog post](https://aws.amazon.com/blogs/containers/operating-a-multi-regional-stateless-application-using-amazon-eks/) demonstra como você pode manter um cluster de Amazon EKS multi-region e pode servir como base caso você necessite de um cenário active-failover ou active-active:

![EKS cross-region](/images/eks-cluster-multi-region.jpg)