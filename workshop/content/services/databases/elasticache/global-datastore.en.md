---
title: "ElastiCache - Global Datastore"
menuTitle: "Global Datastore"
tags:
    - Warm Standby
awsServices:
 - Amazon ElastiCache
---


Global Datastore for Redis functionality enables you to work with fully managed cross-region replication quickly, reliably and securely. Each Global Datastore is a collection of one or more cluster that replicate data with each other. To use cross-region replication, you must have at least 2 clusters (**Primary** and **Secondary**, or **Active** and **Passive**, respectively) configured. The primary cluster is read/write, while the cluster **secondary** it's just read and must be in **another region**.

For the Disaster Recovery strategy, if the primary cluster shows signs of degradation, a secondary cluster can be promoted to a primary cluster. The diagram below illustrates its operation:

![Global Datastore](/images/Global-DataStore.png)

{{< youtube xx6X5s9uLBs >}}

**RTO:** 1 minute

**RPO:** Generally less than 1 second

***

**References:**

*   [Global Datastore - Official Documentation](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Redis-Global-Datastore.html)
*   [Building a Global Solution with Amazon ElastiCache for Redis Global Datastore](https://www.youtube.com/watch?v=xx6X5s9uLBs)
