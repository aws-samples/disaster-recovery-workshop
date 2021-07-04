---
title: "AWS Global Infrastructure"
menuTitle: "Global Infrastructure"
weight: 30
chapter: false
pre: "<b>3. </b>"
---

For the work of planning disaster recovery strategies, it's important to know the global AWS infrastructure. It is necessary to use at least 2 regions to avoid impacts of disrupting services at the region level. View the AWS infrastructure map and choose the regions that will be used by your applications.
Go to [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/).

{{% notice tip%}} <i class="fas fa-lightbulb"></i>
The regions are geographically distributed and this reflects the time of communication between them. If your application is latency sensitive, you should balance the proximity of regions with the possibility of a regional event.
{{% /notice%}}

There are services that are not available in all regions. When choosing a region, confirm that the service you want to use is available. See the table for [AWS Regional Services](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/).

---
**References:**

- [AWS Global Cloud Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)
