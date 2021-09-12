---
title: "Amazon FSx for Windows File Server data across AWS Regions"
menutitle: "Amazon FSx for Windows File Server data across AWS Regions"
weight: 1
pre: "<b></b>"
hidden: true
---

#### Log into the AWS console




** **[Click here to access AWS Management Console:](https://console.aws.amazon.com/)** **


#### Step 1: Set up Amazon FSx for Windows File Server in your source Region

Assuming you don’t already have Amazon FSx setup, the first step is to create an Amazon FSx for Windows File Server file system, which is the storage for your files. You can join the file system to your existing Active Directory. By joining the file system to your Active Directory, the file permissions can be set using your users and groups in Active Directory.

AWS has created a **[getting started](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/getting-started.html)** guide (or **[video](https://www.youtube.com/watch?v=IMDWTIShlyI&feature=youtu.be)** if you would prefer to watch) that walks you through the process of creating an Amazon FSx file system.

For this lab, was created an Amazon FSx file system that spans multiple **[AWS Availability Zone](https://aws.amazon.com/about-aws/global-infrastructure/regions_az/)**  (Multi-AZ) with 32 GiB of capacity and 16 MB/s of throughput capacity in the N. Virginia Region. When you create the file system, Amazon FSx allows you to specify: <br>

* The **Deployment type** (Single-AZ or Multi-AZ)<br>
    * You may have requirements that require you to have your data available if an Availability Zone (AZ) failure occurs, or to remain online during an Amazon FSx maintenance period. If so, we recommend that you deploy a Multi-AZ architecture. The Multi-AZ architecture instructs Amazon FSx to do block level replication of your data from one AWS Availability Zone to a second Availability Zone. During an Amazon FSx maintenance period, Amazon FSx fails over to the other Availability Zone and performs maintenance on the server in the primary Availability Zone. The service then switches and perform maintenance on the server in the second Availability Zone.<br>

* The **Storage type** (SSD or HDD)<br>
* **Storage Capacity**<br>
* **Throughput Capacity**<br>
    * To help you set throughput performance, the service provides a recommended throughput capacity based upon the storage capacity that you set. However, you still have the option to customize it for your application’s specific needs.<br>
* The Window authentication mode (AWS Managed Microsoft Active Directory or Self-managed Microsoft Active Directory)<br>
    * If you have an existing on-premises Active Directory or self-managed in the cloud Active Directory, then you would select the **Self-managed Microsoft Active Directory**<br>
    * If you are using AWS Managed Microsoft AD and would like to join Amazon FSx to that AD, then select the **AWS Managed Microsoft Active Directory**<br>
    * The following screenshot shows all of the specifications already selected for the purposes of this example.<br>


![fsx](/images/fsx-win-step1.png)


### Step 2: Enable communications between AWS Regions


There are two parts to enabling communications between the source AWS Region and the DR AWS Region: (1) enabling network communications, and (2) modifying AWS security groups to allow communications.

Network communications must be enabled between AWS Regions. There are two common methods for enabling inter-Region communications: **[Virtual Private Cloud (VPC) peering](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html)** or **[AWS Transit Gateway](https://aws.amazon.com/transit-gateway/?whats-new-cards.sort-by=item.additionalFields.postDateTime&whats-new-cards.sort-order=desc)**. VPC peering is good if you have a few AWS networks to connect. If you have to connect multiple AWS networks (or VPCs) together, then AWS Transit Gateway is typically recommended.

In this lab , was used VPC peering since was only needed to connect or peer the two VPCs together: the VPC in the source Region and the VPC in the DR Region. For a step-by-step guide to setting up VPC peering, please see the **[documentation](https://docs.aws.amazon.com/vpc/latest/peering/working-with-vpc-peering.html)**.

Each Amazon FSx for Windows File Server has a security group that controls what hosts (IP addresses) are allowed to access it. Update the security group protecting the Amazon FSx file system in the source Region by adding the IP address range for the DR AWS Region. Also, after creating the Amazon FSx file system in the DR Region, you must modify its security group to allow communications from the IP address range in the source AWS Region.


### Step 3: Set up Amazon FSx for Windows File Server in your DR Region

The second step is to create an Amazon FSx for Windows File Server file system in the DR AWS Region. You can follow similar steps as you did to create the Amazon FSx file system in the source Region. However, if you are using the **[AWS Management Console](https://aws.amazon.com/console/)**, change the AWS Region that you are operating in to the Region where you want to create the DR Amazon FSx file system. Do this before you start creating the Amazon FSx file system

You should join the file system to the same Active Directory as the source Amazon FSx file system so that the file permissions are the same. The result is that your users have the same file access.

For the Amazon FSx file system in the DR Region, you may decide to use the same specifications as your source Amazon FSx file system (for example, Multi/Single-AZ, storage type, storage size, and throughput). Depending upon your requirements, you may decide to use a different specification. The previous factors (Multi/Single-AZ, storage type, storage size, and throughput) drive the cost of running Amazon FSx. At a minimum, you should match the storage sizes of the source and DR file systems so that the DR file system can hold the data in the source file system. For more information on Amazon FSx pricing, please see the **[documentation](https://aws.amazon.com/fsx/windows/pricing/)**.

### Step 4: Setting up AWS DataSync agent near your source data

The next step is to install the **[DataSync agent](https://docs.aws.amazon.com/datasync/latest/userguide/working-with-agents.html)**. AWS recommends installing the AWS DataSync agent as close in network connectivity to the source file system. The AWS DataSync agent reads the data from the source system and copies it to the AWS DataSync service in an efficient, high-speed manner. AWS provides an image that has this agent deployed on it. To install the AWS DataSync agent, log in to the AWS Management Console and navigate to the AWS DataSync service. Select the **Get started** button.

Currently, as shown in the following screenshot, AWS provides two options for deploying the agent, either a VMware image or an EC2 image. To support replication from one AWS Region to another AWS Region, we are to use the Amazon EC2 image.

![fsx](/images/fsx-win-step4.png)

You can select how you want to communicate from the DataSync agent to the DataSync service by selecting the service endpoint. The two most common options are to communicate over the internet using the public service endpoints or over a private connection using the **[VPC endpoints using AWS PrivateLink](https://docs.aws.amazon.com/datasync/latest/userguide/datasync-in-vpc.html)**.

For AWS DataSync transfers between AWS Regions, you would most likely select the private connection using VPC endpoints to keep the replication traffic over a private connection. However for brevity of this lab, it's not necessary add the complexity of creating the VPC endpoints for this private connection (in this case). Thus, was selected the public service endpoints (see the following screenshot). However, if you want to use the private connection using VPC endpoints, please see our **[documentation](https://docs.aws.amazon.com/datasync/latest/userguide/datasync-in-vpc.html)**.


![fsx](/images/fsx-win-step4_1.png)

Once you deploy the agent, you must register the agent with the DataSync service by entering the name or IP address of the agent. Then press the Get key button.

![fsx](/images/fsx-win-step4_2.png)

To active the DataSync agent, make sure that the AWS Management Console can communicate with the DataSync agent. Once the DataSync agent registers successfully, you should see it listed in the console:

![fsx](/images/fsx-win-step4_3.png)

*For more on installing the DataSync agent information, please see the **[documentation](https://docs.aws.amazon.com/datasync/latest/userguide/configure-agent.html)**.*


### Step 5: Create the DataSync data replication task

The last step is to create the data replication task where you configure the source location, destination location, and migration settings. It is important to create the DataSync migration task in the same AWS Region as the target storage location (for example, in my scenario in the Oregon Region).

Open the AWS Management Console, and from the Region drop-down menu in the top right of the console, select the AWS Region where the DR Amazon FSx file system is created. Then navigate to the DataSync console and select the **Create task** button.

**Step 5.1: Specify the source location**

On the Configuration screen, you specify the source location options. For the Agent setting, select the DataSync agent that you installed in the prior step. Since you are replicating data from Amazon FSx, it is considered SMB file storage, so select the **Server Message Block (SMB)** option. For the SMB Server IP address, specify the **Preferred File Servier IP Address**, which can be found on the properties page of the Amazon FSx file system:

![fsx](/images/fsx-win-step5_1.png)

Specify the Windows file share on the server that you want to replicate. In my lab, my source Windows file server was at 10.0.22.151 and the **Share name** is share.

![fsx](/images/fsx-win-step5_12.png)

Next, specify the credentials of a user that has rights to read the data from the source location. A typical implementation is to create a service account that is a member of the Windows backup operators group and specify that service account. In this scenario, the source location is Amazon FSx, so make sure that the service account is a member of the **AWS Delegated FSx Administrators** group. For more information, please see our **[documentation](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/limit-access-file-folder.html)**. The following screenshot shows all of the specifications already selected for the purposes of this example.

![fsx](/images/fsx-win-step5_13.png)

**Step 5.2: Specify the destination location**

For this step, specify the destination location where the data should be migrated. Under **Configure destination location** for the location type, specify the **Amazon FSx for Windows File Server** option. Select the Amazon FSx file system in the DR AWS Region that you created earlier and the share name where you want to copy the data.

![fsx](/images/fsx-win-step5_2.png)

You must also specify an account that has rights to write data to the Amazon FSx file system. To ensure sufficient permissions to files, folders, and file metadata, we recommend that you pick a user that is a member of the Amazon FSx file system’s delegated administrators group. For more information, please see our **[documentation](https://docs.aws.amazon.com/datasync/latest/userguide/create-fsx-location.html)**.

![fsx](/images/fsx-win-step5_22.png)

**Step 5.3: Specify the DataSync task settings**

On the next screen, you set the DataSync task settings, as shown in the following screenshot. Here are a couple suggestions on settings these items:

Since you are replicating the data to the DR AWS Region, verification of all the data in the destination every time the replication task is run is not required. Choose **Verify only the data transferred**. This change instructs AWS DataSync task to verify only the data transferred. For more information on each option, please see our **[user guide](https://docs.aws.amazon.com/datasync/latest/userguide/create-task.html)**.

![fsx](/images/fsx-win-step5_31.png)

* On the **Schedule** option (shown in the following screenshot), you can specify when you want the AWS DataSync task to run. For more information, please see our **[user guide](https://docs.aws.amazon.com/datasync/latest/userguide/task-scheduling.html)**.

![fsx](/images/fsx-win-step5_32.png)

**Step 5.4: Run the DataSync migration task**

Once you have specified the task setting and created the DataSync task, it then runs on the schedule that you specified. If you want to start the task immediately, you can do so by selecting the DataSync task and under **Actions**, select **Start**.


![fsx](/images/fsx-win-step5_41.png)

Now to check that the DataSync replication task is operating, logon to a Windows Server that has access to the Amazon FSx file system in the source and DR AWS Regions. Map a file share to the Amazon FSx file system in the DR AWS Region. If you need help with mapping a file share to the Amazon FSx file system, please see our **[documentation](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/using-file-shares.html)**.

You should see your files appearing on the Amazon FSx file system as the AWS DataSync task copies the files. In the following screenshot, the file explorer on the left is my source file system and the one on the right contains the files on the Amazon FSx file system.

![fsx](/images/fsx-win-step5_42.png)











