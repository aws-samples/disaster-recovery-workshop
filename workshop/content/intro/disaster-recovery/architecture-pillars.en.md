---
title: "Pillars for Resilient Workloads"
menuTitle: "Resilient Workloads"
weight: 3
chapter: false
pre: "<b></b>"
type: documentation
---

There are 3 crucial pillars for *workloads* run in the environment *Cloud* be resilient since their conception. These pillars base the architectural decisions that enable the movement of *workload* flexibly and securely: <br><br>

|Infrastructure-AS-Code|Data Movement | Application Architecture |
|-|-|-|
| Ensure your architecture is replicable in an automated way, making it possible to create and recreate this environment elsewhere, as well as giving visibility to changes through code versioning tools such as [Git](https://git-scm.com/) | Data is serious. This means that the trend is that all the services required to manipulate this data are as close as possible to your repository. Questions such as latency, volumetry, costs and way of access to this data should be considered, planning specific forms of movement if necessary. | Conceive *workloads* that ensure low coupling between components, do not maintain state, communicate asynchronously and ensure the persistence and integrity of data in external repositories. In this way, it is possible for your application to move without impact to users or data being manipulated |
| **References:**<br> [Infrastructure-as-Code](https://docs.aws.amazon.com/whitepapers/latest/introduction-devops-aws/infrastructure-as-code.html), [Management and Governance]({{< ref "/services/management">}} "Management and Governance") | **References:**<br>[Storage]({{< ref "/services/storage" >}} "Storage"), [Database]({{< ref "/services/databases" >}} "Database"), [Networks and Content Delivery]({{< ref "/services/networking" >}} "Networking and Content Delivery") |**References:**<br> [Modern Application Architectures](https://www.youtube.com/watch?v=wD_uaqCu5Mc), [Compute]({{< ref "/services/compute" >}} "Compute"), [Containers]({{< ref "/services/containers" >}} "Containers"), [Application Integration]({{< ref "/services/networking" >}} "Application Integration") |
