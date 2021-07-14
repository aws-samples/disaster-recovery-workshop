---
title: "EC2 - Copying an AMI to Another Region"
menutitle: "AMIs Copy"
weight: 1
chapter: true
pre: "<b></b>"
awsServices:
 - Amazon EC2
tags: 
 - Backup & Restore
---

You can copy an Amazon Machine Image (AMI) into or to another AWS Region using the AWS Management Console, the AWS Command Line Interface, or SDKs, or the Amazon EC2 API, all of which support the CopyImage action. You can copy Amazon EBS-based AMIs and Instance Store AMIs. You can copy AMIs with encrypted snapshots and also change the encryption status during the copy process.
Copying a source AMI results in an identical target AMI but with its own unique identifier. In the case of an AMI with Amazon EBS, each of its supporting snapshots is by default copied to an identical but distinct target snapshot. (The only exceptions are when you choose to encrypt the snapshot or encrypt it again.) You can change or deregister the source AMI without affecting the destination AMI. The reverse is also true.
There is no charge to copy an AMI. But standard data transfer and storage fees apply. If you copy an EBS-based AMI, you will be charged for storing additional EBS snapshots.

The following diagram shows the relationships between a source AMI and two AMIs copied in different regions, as well as the EC2 instances launched from each. When you launch an instance from an AMI, it will reside in the same region where the AMI resides. If you make changes to the source AMI and want these changes to be reflected in the AMIs of the destination regions, you must recopy the source AMI to the destination regions.

![AMI Copy](/images/ec2-ami-copy.png)

When you first copy an Instance Store AMI to a region, we'll create an Amazon S3 bucket for the AMIs copied to that region. Any instance store AMIs that you copy to that region will be stored in that bucket. The bucket names have the following format: *amis-for-account-in-region-hash*. For example: *amis-for-123456789012-in-us-east-2-yhjmxvp6*.

See the procedure in this [blog post](https://aws.amazon.com/blogs/aws/ec2-ami-copy-between-regions/).

---

**References**

- [Amazon EC2 - Copying an AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/CopyingAMIs.html)
- [Amazon EC2 - Copy of AMI to Other Regions](https://aws.amazon.com/premiumsupport/knowledge-center/copy-ami-region/)
