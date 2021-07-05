---
title: "Amazon RDS"
menutitle: "Amazon RDS"
weight: 1
chapter: true
pre: "<b></b>"
awsServices: "Amazon RDS"
---

Amazon Relational Database Service (Amazon RDS) is a web service that makes it easy to set up, operate, and scale a relational database in the AWS Cloud. It provides cost-effective, scalable capacity for an industry-standard relational database and manages common database administration tasks.

**DB Instances**

The basic building block for Amazon RDS is the database instance. A DB instance is an isolated database environment on the AWS Cloud. The DB instance can contain several user-created databases. You can access the DB instance using the same tools and applications used with a standalone DB instance. Create and modify a DB instance using the AWS Command Line Interface, the Amazon RDS API, or the AWS Management Console.

Each DB instance runs a database engine. Amazon RDS currently supports MySQL, MariaDB, PostgreSQL, Oracle, and Microsoft SQL Server database engines. Each database engine has its own supported features, and each version of a database engine can include specific features. In addition, each database engine has a set of parameters in a database parameter group that control the behavior of the databases it manages.

For more information see the [official documentation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html).

---
**References:**
- [Relational databases: Performance,scale, and availability](https://d1.awsstatic.com/events/reinvent/2019/Relational_databases_Performance,_scale,_and_availability_FSI309.pdf)
