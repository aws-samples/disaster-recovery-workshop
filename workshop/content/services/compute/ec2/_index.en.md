---
title: "Amazon EC2"
menutitle: "Amazon EC2"
weight: 1
chapter: true
pre: "<b></b>"
---


Amazon Elastic Compute Cloud (Amazon EC2) is a web service that provides secure, scalable computing capacity in the cloud. It's designed to facilitate web-scale cloud computing for developers. Amazon EC2's simple web service interface allows you to get and configure capacity without much effort. It gives you complete control of your computing resources and allows you to work in Amazon's proven computing environment.

## What is AMI?

An Amazon Machine Image (AMI) is a model that contains a software configuration (for example, operating system, application server, and applications). From an AMI, launch an instance, which is a copy of the AMI that runs as a virtual server in the cloud. You can launch multiple instances of an AMI, as shown in the following figure.

![EC2 - AMI](/images/ec2-instances-ami.png)

Your instances will continue to launch until you stop, hibernate, or shut down, or until they fail. If an instance fails, you can launch a new instance from the AMI.

## AMI Types

You can select an AMI to use based on the following characteristics:

*   Region (see [Regions and Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html))
*   Operating System (Windows, Linux, and MacOS)
*   Architecture (32-bit or 64-bit)
*   [Run Permissions](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ComponentsAMIs.html#launch-permissions)
*   [Storage for the root device](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ComponentsAMIs.html#storage-for-the-root-device)

---

**References**

- [Amazon EC2 - Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instances-and-amis.html)
- [Amazon EC2 - AMI Types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ComponentsAMIs.html)
