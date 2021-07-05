---
title : "Amazon SQS - Pilot Light"
menuTitle : "Pilot Light"
weight : 5
tags:
 - Pilot Light
awsServices:
 - Amazon SQS
---


Serverless features such as SQS queues can easily support a Pilot Light architecture through the use of a multi-region infrastructure delivery pipeline using, for example, the Serverless Application Model (SAM) framework. Every new feature or change to existing resource passes through an integration and delivery pipeline for validation and deployment in more than one region. Once it is necessary to activate the new region, simply key the requisitions to the new region by making use of existing resources that do not offer cost when no longer demand.

---

**References:**

- [Amazon SNS Cross Region Delivery to SQS Queues](https://docs.aws.amazon.com/sns/latest/dg/sns-cross-region-delivery.html)
- [AWS Serverless Application Model](https://aws.amazon.com/serverless/sam/)
