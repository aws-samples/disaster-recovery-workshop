---
title: "Amazon S3 - Bi-Directional Cross-Region Replication (CRR)"
menutitle: "S3 - Bi-Directional Replication"
chapter: false
tags:
  - Active-Active
type: Lab
awsServices: 
  - Amazon S3
---

### Objective

This exercise will show you the steps for using object replication through Amazon S3 bucket AWS Regions (*Cross region replication*). If you want more information [click here]({{< ref "/services/storage/s3/s3-crr" >}} "Amazon S3").

**By the end of this exercise, you will be able to:**

- Perform Auto-Replication of Objects in Amazon S3 Buckets
- Automate Disaster Recovery (DR) for Applications Using Objects in Amazon S3
- Search for logs on CloudTrail to improve understanding how cross-region replication works.

**Estimated Duration:** 20 minutes

**Approximate Cost**: 1 USD

### Introduction

This lab will guide you to increase the resilience of applications that rely on objects in Amazon S3. The functionality of [Amazon S3 replication (CRR)](https://docs.aws.amazon.com/pt_br/AmazonS3/latest/dev/replication.html), allows asynchronous copying to another AWS Region.

![S3 Lab Architecture](/images/s3-bi-crr.png?width=550px")

### Execution
{{< tabs name="labs_types" >}} 
{{< tab name="AWS Console" include="console" />}} 
{{< /tabs >}}

---

#### Author

- Seth Eliot, Principal Reliability Solutions Architect, AWS Well-Architected

**Source:** [Amazon S3 - Implementing Bi-directional Cross-Region Replication (CRR)](https://wellarchitectedlabs.com/reliability/200_labs/200_bidirectional_replication_for_s3/)
