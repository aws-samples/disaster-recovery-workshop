---
title: "Aurora - Global Database"
menutitle: "Global Database"
chapter: false
pre: "<b></b>"
awsServices: 
    - Amazon Aurora
tags:
    - Warm Standby
---

## Overview of Aurora Global Databases

An Aurora global database consists of a primary AWS region where data is maintained and up to five read-only AWS secondary regions. Aurora replicates the data to the AWS Secondary Region with typical latency of less than a second. You issue write operations directly to the primary DB instance in the AWS Primary Region.

An Aurora global database uses dedicated infrastructure to replicate your data, leaving database resources fully available to serve application workloads. Applications with a worldwide presence can use reader instances in AWS secondary regions for low latency reads. In some unlikely cases, the database may be degraded or isolated in an AWS Region. If this happens, with a global database, you can promote one of the AWS secondary regions to perform full read/write workloads in less than a minute.

**Amazon Aurora Global Databases Deep Dive:**
{{< youtube 1vFg1z-2E7Y >}}

**RTO:** 1 minute.

---

**References:**

- [Aurora - Global Database](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-global-database.html#aurora-global-database-overview)
- [Aurora - Global Database Lab (MySQL)](https://awsauroralabsmy.com/global/deploy/)
- [AWS Docs - Global Databases](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-global-database.html)
- [Global Databases (PostgreSQL)](https://aws.amazon.com/blogs/database/aurora-postgresql-disaster-recovery-solutions-using-amazon-aurora-global-database/)
- [Write Forwarding (MySQL)](https://aws.amazon.com/blogs/database/building-globally-distributed-mysql-applications-using-write-forwarding-in-amazon-aurora-global-database/)
