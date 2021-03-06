---
title : "Amazon SQS - Backup & Restore"
menuTitle : "Backup & Restore"
weight : 5
tags:
  - Backup & Restore
awsServices:
  - Amazon SQS
---

In general, considering the concept of message queues where we have message producers and consumers, in the ideal world, the depth of a queue should always tend to zero messages since consumers immediately consume all the messages generated by producers. However, there are other reasons that may lead to the accumulation of messages such as:

* Need message flow control to avoid processing overhead on the backend
* Using queues for temporary storage for scheduled batch processing
* DLQ queue that hold messages that could not be processed for consumer error or poison pill scenario

Considering these cases, it is important to think of alternatives to store these messages in a scenario of service unavailability in a region.
One approach to allow message sharing with other regions easily is to use [SQS Extended Client](https://github.com/awslabs/amazon-sqs-java-extended-client-lib). With it, you can send all messages to S3 using a simple setup. This feature is quite useful for handling messages that are larger than [maximum supported by Amazon SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/quotas-messages.html).
