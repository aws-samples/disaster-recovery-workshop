---
title: "Amazon DynamoDB - Global Tables"
menutitle: "Global Tables"
chapter: false
pre: "<b></b>"
awsServices:
 - Amazon DynamoDB
tags: 
 - Active-Active
---

## Global Tables

Global table is the collection of one or more replica tables, all belonging to a single AWS account.

Replica table (or just replica) is a single DynamoDB table that works as part of a global table. Each replica stores the same set of data items. Any given global table can only have one replica table per AWS region.

When creating a global DynamoDB table, it consists of several replica tables (one per region) that DynamoDB treats as a single unit. Each replica has the same table name and the same primary key schema. When an application writes data to a replica table in a region, DynamoDB propagates the write to the other replica tables in other AWS regions automatically.

{{< youtube k9_9bDZa_EI>}}

## Consistency and Conflict Resolution

An application can read and write data to any replica table. If your application uses only eventually consistent reads and only problem reads in an AWS Region, it will work without any modification. However, if the application requires strongly consistent reads, it should perform all of its strongly consistent reads and writes in the same region. DynamoDB doesn't support strongly consistent cross-region reads. Therefore, if you write to one region and read in another, the response read might include stale data that doesn't reflect the results of newly completed writes in the other region.

*Excerpt taken [Amazon DynamoDB Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/V2globaltables_HowItWorks.html)*

---
**References:**
- [AWS Developer Workshop: How to Build Multi-Region Applications in the Cloud](https://www.youtube.com/watch?v=k9\_9bDZa_EI)
- [AWS Solutions: Multi-Region Availability with Amazon DynamoDB, Amazon S3, and Amazon Cognito (LIVE)](https://www.youtube.com/watch?v=tTQ36qQF_vA)
- [AWS Re:Invent 2018: How Oath Built a Multi-Region GDPR Application with Amazon DynamoDB (DAT325)](https://www.youtube.com/watch?v=HCfoKyoimOg)
