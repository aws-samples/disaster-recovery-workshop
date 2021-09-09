---
title: "Amazon ElastiCache for Redis - Global Datastore"
menutitle: "ElastiCache - Global Datastore"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Warm-Standby]
type: Lab
awsServices:
 - Amazon ElastiCache
---

### Objective

This exercise will show you the steps for using the Global Datastore mechanism for Amazon ElastiCache instances. If you want more information [click here]({{< ref "/services/databases/elasticache/global-datastore" >}} "Amazon ElastiCache").

**By the end of this exercise, you will be able to:**

*   Create Amazon ElastiCache for Redis
*   Replicate Cluster to Secondary Region
*   Test Amazon ElastiCache across regions
*   Promote Secondary Region to Primary

**Estimated Duration:** 40 minutes

**Approximate Cost**: 10 USD

### Execution
{{< tabs name="labs_types" >}} 
{{< tab name="AWS Cloud9" include="cloud9" />}}  
{{< /tabs >}}

### Conclusion

With this exercise, it was possible to verify how use Global Datastore to replicate data of Amazon ElastiCache for Redis across regions. Global Datastore instances can be used in a strategy of the type *Warm Standby*, in order to implement disaster recovery capabilities improving the availability.

**References:**
- [Amazon ElastiCache for Redis - Global Datastore](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Redis-Global-Datastore.html)
- [Configuring Amazon ElastiCache for REdis for higher availability](https://aws.amazon.com/blogs/database/configuring-amazon-elasticache-for-redis-for-higher-availability/)
- [Now Available: Amazon EastiCache Global Datastore for Redis](https://aws.amazon.com/blogs/aws/now-available-amazon-elasticache-global-datastore-for-redis/)




