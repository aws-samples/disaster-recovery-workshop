---
title: "ElastiCache - Backup & Restore"
menuTitle: "Backup & Restore"
tags:
 - Backup & Restore
awsServices:
 - Amazon ElastiCache
---

You can backup ElastiCache clusters for Redis only. Memcached Clusters don't have this feature. The cluster configuration defines the strategy for automated daily backups with a maximum retention period of 35 days. Backups use Redis's native strategy, generating .rdb files that can be exported to an S3 bucket and then ported to another region (through S3 cross-region replication, for example) if necessary.

It is recommended to plan the backup strategy by setting the execution window for the period with the lowest load expected on the day. Manual daytime backups impact cluster performance. To increase backup performance, consider adjusting the parameter *reserved-memory-percent* to minimize overpagination. Also consider running backups from *read-replicas*, minimizing competition for writing data to the cluster.

Once the backup is exported, you can create a new cluster using the .rdb file generated in any region where Amazon ElastiCache is available. Once the backup is restored, the Redis cluster will be *warmed up* and ready to receive requests.

**RPO:** 1 day (automatic backup). Less than 1 day (manual backups) - Performance, overload, and runtime should be considered

**RTO:** Dependent on backup size

---
**References:**

- [Backup and Restore for ElastiCache for Redis](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/backups.html)
- [Improving Backup Performance](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/backups.html#backups-performance-improving)
- [Redis memory management](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/redis-memory-management.html)
- [S3 replication](https://docs.aws.amazon.com/AmazonS3/latest/dev/replication.html)
