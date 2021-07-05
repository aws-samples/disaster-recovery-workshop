---
title: "Amazon S3"
weight: 2
chapter: true
pre: "<b></b>"
awsServices: 
    - Amazon S3
---


Amazon Simple Storage Service is storage for the Internet. It is designed to facilitate scaling computing on the web for developers.

Amazon S3 has a simple web services interface that you can use to store and retrieve any amount of data, anytime, anywhere on the web. It gives all developers access to the same highly scalable, reliable, secure, fast, and cost-effective infrastructure that Amazon uses to run its own global network of websites. The service aims to maximize the benefits of scale and be able to pass these benefits to developers.

## Amazon S3 Data Consistency Model

Amazon S3 provides strong read-after write consistency for PUTs and Deletes of objects to the Amazon S3 bucket across AWS regions. This applies to both writes to new objects, as well as PUTs that overwrite existing objects and Deletes. In addition, read operations in Amazon S3 Select, Amazon S3 Access Control Lists, Amazon S3 Object Tags, and Object Metadata (for example, HEAD object) are strongly consistent.

Updates in a single key are atomic. For example, if you perform PUT on an existing key on a thread and run a GET on the same key from a second thread simultaneously, you will get the old data or new data, but never partial or corrupt data.

Amazon S3 achieves high availability by replicating data across multiple servers in AWS data centers. If a PUT request is successful, the data will be stored securely. Any read (GET or LIST) initiated after receiving a successful PUT response will return the data written by PUT. Here are some examples of this behavior:

*   A process writes a new object to Amazon S3 and immediately lists the keys in your bucket. The new object will appear in the list.

*   A process replaces an existing object and immediately tries to read it. Amazon S3 will return the new data.

*   A process deletes an existing object and immediately tries to read it. Amazon S3 will not return data as the object was deleted.

*   A process deletes an existing object and immediately lists the keys in your bucket. The object will not appear in the listing.

{{% notice note%}}

Amazon S3 does not support object blocking for concurrent writers. If two PUT requests are performed simultaneously on the same key, the request with the most recent time stamp will be chosen. If this is a problem, you'll need to create an object lock mechanism in your application.
{{% /notice%}}

{{% notice note%}}
The updates are key based. There is no possibility to perform atomic updates between keys. For example, you can't make updating a key dependent on updating another key unless you develop that functionality in your application.

{{% /notice%}}

---
**References:**
- [S3 - Consistency Model](https://docs.aws.amazon.com/AmazonS3/latest/dev/Introduction.html#ConsistencyModel)
- [LAB: S3 - Two-Way Replication]({{< ref "bidire-rep-s3" >}})
