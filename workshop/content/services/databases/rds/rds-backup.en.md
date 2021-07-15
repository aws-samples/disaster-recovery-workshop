---
title: "RDS - Cross-Region Backups"
menutitle: "Cross-Region Backups"
chapter: false
pre: "<b></b>"
awsServices: 
 - "Amazon RDS"
tags:
 - "Backup & Restore"
---


Amazon RDS creates and saves automated backups of your DB instance during the backup window of your DB instance. RDS creates a snapshot of the DB instance storage volume, backing up the entire DB instance and not just the individual databases. RDS saves automated backups of the DB instance according to the specified backup retention period. If necessary, you can recover your database at any point in time during the backup retention period

Amazon RDS backup storage for each region consists of automatic backups and manual database snapshots for that region. The total backup storage space equals the sum of storage for all backups in that region. Moving a database snapshot to another region increases backup storage in the destination region. The backups are stored on Amazon S3. You can copy a snapshot within the same AWS Region, a snapshot across AWS Regions, and shared snapshots.

Using AWS Backup, you can copy backups to multiple AWS regions on demand or automatically as part of a scheduled backup plan. Cross-region replication is particularly valuable if you have business continuity or compliance requirements to store backups at a minimum distance from production data.

{{< youtube 7gNXfo5HZN8 >}}

---
**References:**
- [How to copy a cross-region snapshot](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CopySnapshot.html)
- [AWS Backup - Cross-Region Backups](https://docs.aws.amazon.com/aws-backup/latest/devguide/how-it-works-cross-region-replication.html)
- [Replicating automated backups to another AWS Region](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReplicateBackups.html)
- [Automate RDS Snapshots for disaster recovery](https://github.com/aws-samples/ssm-for-rds-snapshot-cross-region-copy)
