---
title: "AWS Regions and Zones"
menutitle: "Regions and Zones"
weight: 1
chapter: false
pre: "<b></b>"
---


Each region is designed to be isolated from other regions. This provides the highest possible fault tolerance and stability.

![Regions](/images/aws-region2.png)

Availability zones are several datacenters isolated in each region.

{{% notice note%}}
AWS operates datacenters state-of-the-art and highly available. While rare, failures may occur that affect the availability of instances and services that are in the same location. If you host all of your instances in a single location affected by a failure, none of them will be available. That's why it's important to consider using multiple availability zones for production applications, and consider using multiple regions for critical applications.

{{% /notice%}}

See the [Regions List](https://docs.aws.amazon.com/general/latest/gr/rande.html#region-names-codes) available on AWS.

---
**References:**

- [Regions and Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-regions)
