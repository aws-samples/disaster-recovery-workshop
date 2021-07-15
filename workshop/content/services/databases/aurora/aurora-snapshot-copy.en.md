---
title: "Aurora - Snapshots Backup"
menutitle: "Snapshots Backup"
chapter: false
pre: "<b></b>"
awsServices: 
    - Amazon Aurora
tags:
    - Backup & Restore
---

Aurora automatically backs up the cluster volume and maintains restore data for the backup retention period. Aurora backups are continuous and incremental so you can quickly restore from any point within the backup retention period. When backup data is being written, there is no impact to the performance or service disruption of the database. You can specify a backup retention period, from 1 to 35 days, when creating or modifying a DB cluster. Aurora backups are stored on Amazon S3.

With Amazon RDS you can copy manual or automated snapshots from the DB cluster. After copying a snapshot, the copy is a manual snapshot.
You can copy a snapshot within the same AWS Region, a snapshot across AWS Regions, and shared snapshots.

You cannot copy a DB cluster snapshot between regions and accounts in a single step. Perform a step for each of these copy actions. As an alternative to copying, you can also share manual snapshots with other AWS accounts. For more information, see [Share a snapshot of the DB cluster](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/USER_ShareSnapshot.html).

You can use AWS Backup to manage your Amazon Aurora cluster backups. AWS Backup can centrally configure backup policies, monitor backup activity, copy a snapshot within and across AWS regions.

---

**References:**

- [Aurora - Copy a DB Cluster Snapshot](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/USER_CopySnapshot.html)
- [Understanding Using Aurora Backup Storage](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-storage-backup.html)
- [AWS Backup - Aurora](https://aws.amazon.com/about-aws/whats-new/2020/06/amazon-aurora-snapshots-can-be-managed-via-aws-backup/?nc1=h_ls)
