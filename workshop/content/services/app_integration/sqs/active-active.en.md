---
title : "Amazon SQS - Active-Active"
menuTitle : "Active-Active"
weight : 5
tags:
  - Active-Active
awsServices: 
  - Amazon SQS
---

In an active-active multi-region workload scenario, that is, different regions receive customer traffic by dividing the full load of the solution, the most common approach is to divide customers by location for access to the nearest region. Within this scenario, features that have multi-region replication can be based on this functionality. Features such as message queues have no auto-replication feature of messages to another region and you need to adopt the best policy according to the scenario.
Regarding queue support infrastructure, using infrastructure such as code and a multi-region delivery pipeline allows you to provision resources and changes easily. Regarding messages trafficked, account should be taken of the possibility of loss of messages and whether the backend system is prepared to process repeated messages (idempotence).
If no message can be lost, you must ensure the persistence of the message on a mechanism that has global replication or one must automate this replication using a Fan-out pattern.

---

**References:**

- [Amazon SNS Cross Region Delivery to SQS Queues](https://docs.aws.amazon.com/sns/latest/dg/sns-cross-region-delivery.html)
