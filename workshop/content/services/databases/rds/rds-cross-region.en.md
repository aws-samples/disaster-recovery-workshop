---
title: "RDS - Cross-Region Replication"
menutitle: "Cross-Region Replication"
chapter: false
pre: "<b></b>"
awsServices: 
 - "Amazon RDS"
tags:
 - Warm Standby
---


Amazon RDS uses the replication functionality integrated into the MariaDB, MySQL, Oracle, PostgreSQL, and Microsoft SQL Server database engines to create a special type of DB instance called a Read Replica from a source DB instance.

A Read Replica may be in a different AWS Region than the primary DB instance region. In these cases, Amazon RDS sets up a secure communication channel between the primary DB instance and the Read Replica. Amazon RDS establishes the AWS security settings required to enable the secure channel, such as adding security group entries.

You can only replicate across AWS Regions when using the following Amazon RDS DB Instances:

*   MariaDB (all versions).
*   MySQL version 5.6 and later.
*   Oracle Enterprise Edition (EE), engine release 12.1.0.2.v10 and versions 12.1 higher, all versions of 12.2 and all versions of 18.0. An Active Data Guard license is required. For information about limitations for Oracle cross-region read replicas, see Replica Requirements for Oracle.
*   PostgreSQL (all versions).

---
**References:**

- [How to work with read replicas](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReadRepl.html)
- [Cross-Region disaster recovery of Amazon RDS for SQL Server](https://aws.amazon.com/blogs/database/cross-region-disaster-recovery-of-amazon-rds-for-sql-server/)
- [Best practices for Amazon RDS for PostgreSQL Cross-region read replicas](https://aws.amazon.com/blogs/database/best-practices-for-amazon-rds-for-postgresql-cross-region-read-replicas/)
- [Implementing a disaster recovery strategy with Amazon RDS](https://aws.amazon.com/blogs/database/implementing-a-disaster-recovery-strategy-with-amazon-rds/)
- [Implementing cross-region disaster recovery using Oracle GoldenGate for Amazon RDS for Oracle](https://aws.amazon.com/blogs/database/implementing-cross-region-disaster-recovery-using-oracle-goldengate-for-amazon-rds-for-oracle/)
- [Create a Read Replica in a Different AWS Region](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReadRepl.html?ref=wellarchitected#USER_ReadRepl.XRgn)
