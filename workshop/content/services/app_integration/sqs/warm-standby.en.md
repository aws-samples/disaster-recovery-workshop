---
title : "Amazon SQS - Warm Standby"
menuTitle : "Warm Standby"
weight : 5
tags:
 - Warm Standby
awsServices:
 - Amazon SQS
---

A Warm Standby strategy for features such as Amazon SQS queues is similar to the [Pilot Light]({{< ref "/services/app_integration/sqs/pilot-light" >}} "Pilot Light") scenario.

Serverless resources are charged as you use, so while there is no traffic in the DR region, there are no costs for existing resources.

---
**References:**

- [Amazon SNS Cross Region Delivery to SQS Queues](https://docs.aws.amazon.com/sns/latest/dg/sns-cross-region-delivery.html)
