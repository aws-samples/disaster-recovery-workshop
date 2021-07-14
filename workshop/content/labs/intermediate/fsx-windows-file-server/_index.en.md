---
title: "Amazon FSx for Windows File Server data across AWS Regions"
menutitle: "Amazon FSx across AWS Regions"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Active-Active]
type: Lab
awsServices:
 - Amazon FSx
draft: true
---


### Introduction
Customers who have special compliance or disaster recovery requirements have asked for the ability to replicate data in Amazon FSx for Windows File Server (Amazon FSx) to another AWS Region. Although AWS Regions and Amazon FSx have multiple tiers of resiliency built-in, replication can protect customer data in the unlikely scenario of a catastrophic loss of an AWS Region.


### Solution Overview


![any](/images/architecture.png)

### Goal

How to set up an architecture to replicate Amazon FSx for Windows File Server data between AWS Regions using AWS DataSync. This process involves the following steps:

Step 1: Setting up Amazon FSx for Windows File Server in your source AWS Region<br>
Step 2: Enabling communications between AWS Regions<br>
Step 3: Setting up Amazon FSx for Windows File Server in the second AWS Region. This second AWS Region was refered in the rest of the lab as the Disaster Recovery (DR) AWS Region.<br>
Step 4: Setting up an AWS DataSync agent near your source data<br>
Step 5: Configuring AWS DataSync service to replicate your data from the source Region to the DR Region<br>






**In the end of this lab, you will be able to:**
- Replicate Amazon FSx for Windows File Server data across AWS Regions




### Execution
{{< tabs name="labs_types" >}} 
{{< tab name="AWS Console" include="console" />}} 
{{< /tabs >}}

### Congratulations!

You have set up AWS DataSync to copy files from the Amazon FSx file system in the source AWS Region to the Amazon FSx file system in the DR AWS Region. During task execution, AWS DataSync examines the source files and only copies the files that have changed. If you would like to watch a walkthrough of the process, please review this [video](https://www.youtube.com/watch?v=YO-i6GIGD1E&feature=youtu.be) (you can skip to 3:37 in the video).

### Cleaning up

If you are no longer using the resources discussed in the lab, we suggest that you clean up the AWS resources that are not needed to avoid incurring unwanted charges. After finishing the proof of concept, you can clean up the resources by deleting the DataSync objects (as in the DataSync agent, DataSync task, DataSync source location, and destination location). Then you must delete the Amazon FSx file system created in the source AWS Region and the DR AWS Region. By doing so, you remove further costs from the resources used in this proof of concept.

### Further enhancements

If you have deployed Microsoft Distributed File System (DFS) during a disaster scenario, you can streamline the cutover to the DR Amazon FSx file system. To do so, change the DFS namespace to point the Amazon FSx file system in the DR Region instead of the Amazon FSx file system in the source AWS Region. If you would like to learn more about integrating Amazon FSx with Microsoft DFS, we have recorded a quick [video](https://www.youtube.com/watch?v=s482kj_xMeE&feature=youtu.be) on how to perform this configuration.

### Conclusion

In this lab, customers were helped to have a copy of their Amazon FSx file system data in another AWS Region for compliance or disaster recovery purposes. Was highlighted how AWS DataSync can be used to continuously replicate data from an Amazon FSx file system in a source AWS Region to another AWS Region. AWS DataSync automates the data replication tasks and is efficient since it only replicates the changes.


----- 

**Fonte:** [Amazon FSx](https://aws.amazon.com/blogs/storage/how-to-replicate-amazon-fsx-file-server-data-across-aws-regions/)




