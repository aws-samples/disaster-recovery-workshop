---
title: "Amazon S3 - Cross-Region Replication"
menutitle: "Cross-Region Replication"
weight: 2
chapter: false
pre: "<b></b>"
awsServices: 
    - Amazon S3
tags:
    - Active-Active
type: documentation
---

## Replication Engine

THE [replication](https://docs.aws.amazon.com/AmazonS3/latest/dev/replication.html) enables automatic asynchronous copy of objects to Amazon S3 buckets. Buckets configured for object replication may be owned by the same AWS account or different accounts. You can copy objects between **different AWS regions** or within the same region.

**Replicate objects in 15 minutes:** You can use S3 Replication Time Control (S3 RTC) to replicate your data in the same AWS Region or in different regions over a predictable period. S3 RTC replicates 99.99% of new objects stored in Amazon S3 in 15 minutes (backed by a service level agreement).

---

**References:**

- [LAB: S3 - Two-Way Replication]({{< ref "bidire-rep-s3" >}})
- [Meeting Compliance Requirements Using S3 Replication Time Control (S3 RTC)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication-time-control.html)
