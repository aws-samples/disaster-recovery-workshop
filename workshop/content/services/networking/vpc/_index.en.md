---
title: "Amazon VPC"
menutitle: "Amazon VPC"
chapter: true
pre: "<b></b>"
awsServices:
 - "Amazon VPC"
type: Documentation
---

Amazon Virtual Private Cloud (Amazon VPC) allows you to run AWS resources on a virtual network that you define. This virtual network resembles a traditional network that you would operate in your data center, with the benefits of using AWS scalable infrastructure.

### Amazon VPC Concepts

Amazon VPC is the network layer of Amazon EC2. If you don't have experience with Amazon EC2, see What is Amazon EC2? in the Amazon EC2 User Guide for Linux Instances for a brief overview.

The following are the main concepts of VPCs:

*   **Virtual Private Cloud (VPC):** a virtual network dedicated to your AWS account.

*   **Subnet:** a range of IP addresses in the VPC.

*   **Route table:** a set of rules, called routes, that are used to determine where network traffic will be directed.

*   **Internet Gateway:** a gateway that you attach to the VPC to enable communication between resources in the VPC and the Internet.

*   **VPC endpoint:** Enables you to privately connect your VPC to supported AWS services and VPC endpoint services developed by PrivateLink without requiring an Internet gateway, NAT device, VPN connection, or AWS Direct Connect connection. Instances in your VPC don't require public IP addresses to communicate with resources in the service. The traffic between your VPC and other services doesn't leave the Amazon network.

---
**References**
- [Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
