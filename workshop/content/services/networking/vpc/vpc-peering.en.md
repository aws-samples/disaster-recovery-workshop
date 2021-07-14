---
title: "Amazon VPC - Peering"
menutitle: "VPC Peering"
weight: 3
chapter: true
pre: "<b></b>"
awsServices:
 - "Amazon VPC"
type: Documentation
---

A VPC pairing connection is a network connection between two VPCs that allows you to direct traffic between them using private IPv4 or IPv6 addresses. Instances in any VPC can communicate with each other as if they were on the same network. You can create a pairing connection between your own VPCs or with a VPC from another AWS account. VPCs can be in different regions (also known as cross-region VPC pairing connection).

![VPC Peering Diagram](/images/vpc-peering-diagram.png)

AWS uses the existing VPC infrastructure to create a paired connection between VPCs; it is not a gateway nor a VPN connection and does not depend on external physical hardware. There is no single point of communication failure or a bandwidth bottleneck.

A VPC pairing connection helps you make data transfer easier. For example, if there is more than one AWS account, you can pair the VPCs between those accounts to create a file sharing network. You can also use a VPC pairing connection to allow other VPCs to access the features you have in one of your VPCs.

You can establish pairing relationships between VPCs between different AWS regions (also called interregion VPC pairing). This allows VPC resources, including EC2 instances, Amazon RDS databases, and Lambda functions that run in different AWS regions to communicate with each other using private IP addresses, without requiring separate gateways, VPN connections, or network equipment. The traffic remains in the private IP space. All interregion traffic is encrypted with no single point of failure or bottleneck in bandwidth. Traffic always stays on the global AWS backbone and never traverses the public internet, which reduces threats such as common breaches and DDoS attacks. Pairing intra-region VPCs provides a simple and cost-effective way to share resources across regions or replicate data for geographic redundancy.

---
**References**
- [Amazon VPC Peering](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html)
