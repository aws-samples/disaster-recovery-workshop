---
title: "Amazon DynamoDB"
menutitle: "Amazon DynamoDB"
chapter: true
pre: "<b></b>"
awsServices: Amazon DynamoDB
---

Amazon DynamoDB is a fully managed NoSQL database service that provides fast, predictable performance with seamless scalability. DynamoDB allows you to transfer the administrative operation and scalability loads from a distributed database so you don't have to worry about hardware provisioning, installation, and configuration, software replication and correction, or cluster scaling. DynamoDB also offers encryption at rest, which eliminates the operational burden and complexity involved in protecting sensitive data.

With DynamoDB, you can create database tables that store and retrieve any amount of data and meet all requested traffic levels. You can also increase or reduce the throughput capacity of tables without downtime or performance degradation. You can use the AWS Management Console to monitor resource utilization and performance metrics.

DynamoDB offers the on-demand backup feature. It allows you to create full backups of your tables for long-term retention and archiving to meet regulatory compliance needs. For more information, see [Backup and Restore on Demand for DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/BackupRestore.html).

You can create backups on demand, as well as enable point-in-time recovery for Amazon DynamoDB tables. Point-in-time recovery helps protect tables from accidental write or delete operations. With point-in-time recovery, you can retrieve the table to any point during the last 35 days. For more information, see [Point-in-time Recovery: How It Works](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/PointInTimeRecovery_Howitworks.html).

### High availability and durability

DynamoDB automatically distributes data and traffic to tables across a sufficient number of servers to handle your throughput and storage requirements while maintaining consistent and fast performance. All data is stored on solid-state disks (SSDs) and is automatically replicated across multiple Availability Zones in an AWS Region, which offers high data durability and integrated availability. You can use global tables to keep DynamoDB tables synchronized across AWS Regions.

---
**References:**
- [Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)
- [AWS Developer Workshop: How to Build Multi-Region Applications in the Cloud](https://www.youtube.com/watch?v=k9\_9bDZa_EI)
- [AWS Solutions: Multi-Region Availability with Amazon DynamoDB, Amazon S3, and Amazon Cognito (LIVE)](https://www.youtube.com/watch?v=tTQ36qQF_vA)
- [AWS Re:Invent 2018: How Oath Built a Multi-Region GDPR Application with Amazon DynamoDB (DAT325)](https://www.youtube.com/watch?v=HCfoKyoimOg)
