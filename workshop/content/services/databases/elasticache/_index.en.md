---
title: "Amazon ElastiCache"
menutitle: "Amazon ElastiCache"
chapter: true
pre: "<b></b>"
---

Amazon ElastiCache is a managed service for in-memory database, compatible with [Memcached](https://memcached.org/) and [Redis](https://redis.io/). It is primarily used as a cache layer for online applications in situations such as access to relational databases and temporary high-frequency data storage, such as user session data for example. You can offload the relational database, decreasing the frequency of access dependent services and enabling microservices to scale without the dependency on data synchronization.

You need to plan what the cache strategy is best suited to the type of workload. When the defined strategy involves large volumes of data, a complete refresh of this data becomes a challenge, which can overwhelm dependent services and causing your workload unavailability. We then need to distribute this cache so that it can have resilience in storing information and respects the TTL settings defined by the workload.

Amazon ElastiCache has two options: [Memcached](https://memcached.org/) and [Redis](https://redis.io/). Use Memcached for simpler implementations when your requirement doesn't involve high data availability. Memcached does not have the Automatic Replication and Failover features.
{{% notice info%}}
Redis should be used as an option for Disaster Recovery because it has the Backup & Restore features and the [Global Datastore](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Redis-Global-Datastore.html), recently released.
{{% /notice%}}

You can implement two of the four Disaster Recovery strategies: [Backup & Restore]({{< ref "tags/backup-restore" >}} "Backup & Restore") and [Warm Standby]({{< ref "tags/warm-standby" >}} "Warm Standby"). We will discuss these strategies next.

---
**References:**
- [Memcached or Redis? Select Engine Guide](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/SelectEngine.html)
- [Fault Tolerance for ElastiCache](https://aws.amazon.com/premiumsupport/knowledge-center/fault-tolerance-elasticache/)
- [Amazon Builders' Library - Challenges and Cache Strategy](https://aws.amazon.com/builders-library/caching-challenges-and-strategies/)
- [Amazon ElastiCache Redis Global Datastore](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Redis-Global-Datastore.html)
