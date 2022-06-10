---
title: "Amazon Aurora - Global Database"
menutitle: "Aurora - Global Database"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Active-Active]
type: Lab
awsServices:
 - Amazon Aurora
---

### Objective

These exercises will show you the steps for using the Global Database engine for Amazon Aurora for MySQL databases. If you want more information <a onclick="window.open ('{{< ref "/services/databases/aurora" >}}', ''); return false" href="javascript:void(0);">click here</a>.

**By the end of this exercise, you will be able to:**

- Create database clusters
- Monitor replication performance
- Perform cross-region failover operation

**Estimated Duration:** 2 hours

**Approximate Cost**: 20 USD

# Create an Aurora Global Database

Amazon Aurora <a href="https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-global-database.html" target="_blank">Global Database</a> is designed for globally distributed applications, allowing a single Amazon Aurora database to cover multiple AWS Regions. It replicates your data without impact on database performance, enables fast local reads with low latency in each region, and provides disaster recovery from outages in a region.

This lab contains the following tasks:

1.  Create the Lab Environment
2.  Create an Aurora Global Cluster

# 1. Create the Lab Environment

 <h4>Access AWS Management Console </h4>

You can access the console at: <a href="https://console.aws.amazon.com/" target="_blank">https://console.aws.amazon.com/</a> or through the single sign-on (SSO) mechanism provided by your organization.

![AWS Management Console Login](/images/aurora-2-login.png?raw=true)

Labs are designed to work in any of the regions where Amazon Aurora compatible MySQL is available. However, not all Amazon Aurora features and capabilities may be available in all currently supported regions.

This lab uses the US East(N. Virginia)/us-east-1 region for the Aurora Global Database primary region and the US West(Oregon)/us-west-2 region for the secondary environment. Verify that US East(N. Virginia)/us-east-1 region is selected in the console. 

![AWS Management Console region select](/images/aurora-2-region-select.png?raw=true)

<h4>Create the lab environment using AWS CloudFormation </h4>

To simplify the initial lab experience, we created basic models for <a href="https://aws.amazon.com/cloudformation/" target="_blank"> AWS CloudFormation </a> that provision the resources required for the lab environment in both regions. These models are designed to deploy a consistent network infrastructure and components used in the lab.

Click  **Launch Stack to create an Aurora Provisioned DB Cluster Automatically**:

[![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png "Launch Stack")](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=auroralab&templateURL=https://aurora-cloudformation-template-080622.s3.amazonaws.com/code/aurora-gdb-primary-cfn-template.yml)


In the field called **Stack Name**, make sure the value `auroralab` is default.

![Create Stack](/images/aurora-2-create-stack-params.png?raw=true)

Go to the bottom of the page, check the box that says: **I acknowledge that AWS CloudFormation might create IAM resources with custom names** and click **Create stack**.

![Create Stack](/images/aurora-2-create-stack-confirm.png?raw=true)

The stacks will take approximately 20 minutes to provision. You can monitor the status on the page **Stack detail**. You can monitor the progress of the stacks creation process by updating the tab **Events**. The last event on the list will indicate `CREATE_COMPLETE` for the stack feature.

![Stack Status](/images/aurora-2-stack-status.png?raw=true)

When the status from both stacks are `CREATE_COMPLETE`, click the tab **Outputs**. The values from **auroralab** will be essential for the completion of the rest of the laboratory. Take a moment to save these values in a location where you have easy access to them during the rest of the lab. The names that appear in the column **Key** are referenced directly in the instructions in subsequent steps, using the parameter format: == \[OutputKey] ==.

![Stack Outputs](/images/aurora-2-stack-outputs.png?raw=true)

Now let's check the access to Session Manager, from where we'll run some commands in the following labs.

* <a onclick="window.open ('{{% relref connect %}}', ''); return false" href="javascript:void(0);">Connect to the Session Manager Workstation</a>

{{% notice warning%}}
Do not proceed without checking access to Session Manager.
{{% /notice%}}

## Create a lab environment in the Virginia Region (us-east-1)

Using multiple regions:

{{% notice note%}}
Due to nature **multi-regional** from a global database, you will often switch between two regions to perform the tasks of this lab. **Be Aware** that you are performing the actions in the appropriate region because some of the features created are very similar between the two regions.
Let's refer to the region where your current DB cluster is deployed, and you've been working so far, as yours **primary region**.
Let's refer to the region where you will deploy the secondary and read-only DB cluster as **secondary region**.
{{% /notice%}}

Click **Launch Stack** below to provision a lab environment in the region **US East (Virginia, us-east-1)** to support the Aurora Global Database.

<a href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=auroralab&templateURL=https://s3.amazonaws.com/ams-stack-prod-content-us-east-1/templates/lab_template.yml&param_deployCluster=No&param_deployML=No&param_deployGDB=Yes&param_isSecondary=Yes" target="_blank"><img src="https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png" alt="Launch Stack"></a>

In the field called **Stack Name**, make sure the value `auroralab` is default. Accept all default values for the remaining parameters.

Go to the bottom of the page, check the box that says: **I acknowledge that AWS CloudFormation might create IAM resources with custom names** and click **Create stack**.

![Create Stack](/images/aurora-cfn-create-stack-confirm.png?raw=true)

The stack will take approximately 10 minutes to be provisioned, you can monitor the status on the page **Stack detail**. You can monitor the progress of the stack creation process by updating the tab **Events**. The last event on the list will indicate `CREATE_COMPLETE` for the stack feature.

![Stack Status](/images/aurora-cfn-stack-status.png?raw=true)

When the stack status is `CREATE_COMPLETE`, click the tab **Outputs**. The values here will be essential for the completion of the rest of the laboratory. Take a moment to save these values in a location where you have easy access to them during the rest of the lab. The names that appear in the column **Key** are referenced directly in the instructions in subsequent steps, using the parameter format: == \[OutputKey] ==.

![Stack Outputs](/images/aurora-cfn-stack-outputs.png?raw=true)

## 3. Create an Aurora Global Cluster

The lab environment that was automatically provisioned for you already has an Aurora MySQL DB cluster, in which you are running the load generator. You will create a global DB cluster using this existing DB cluster, such as **primary**.

{{% notice info%}}
'Global Cluster' vs. 'DB Cluster': What's the Difference? <br> One **DB Cluster** exists in just one region. It's a container for up to 16 **database instances** that share the same storage volume. This is the traditional configuration of an Aurora cluster. Whether you're deploying a provisioned, serverless, or multi-master cluster, you're deploying a **DB cluster** in a single region.
A cluster **global** is a container for several **database clusters**, each located in a different region, which acts as a cohesive database. One **global cluster** consists of a **\[DB] primary cluster** in a particular region that is capable of accepting writes and up to 5 clusters **\[DB] secondary** which are read-only, each in a different region. Each of **database clusters** in a given **global cluster** has its own storage volume, but the data is replicated from **primary cluster** for each of **secondary clusters** asynchronously, using a low-latency, high-throughput replication system developed for this purpose.
{{% /notice%}}

Once the lab environment created in step **1. Create a Lab Environment in the Ohio Region (us-east-2)** be finalized, we can continue.

Open the <a href="https://console.aws.amazon.com/rds/home?region=us-east-2#database:id=auroralab-mysql-cluster;is-cluster=true" target="_blank">Amazon RDS service console</a> on the MySQL DB cluster details page in the region **primary**. If you navigated to the RDS console by means other than the link in this paragraph, click `auroralab-mysql-cluster` in the section **Databases** from the RDS service console, and make sure you are back in the primary region.

{{% notice note%}}
Make sure you're still working on **primary region**, especially if you're following the links above to open the service console on the right screen.
{{% /notice%}}

<!---
First, you need to **disable** the resource **Backtrack**. Currently, the database backtrack is not compatible with Aurora global databases, and a cluster with this active resource cannot be converted to a global database. Select the `auroralab-mysql-cluster` and click the button **Modify**.

![RDS Cluster Modify](/images/aurora-rds-cluster-action-modify.png?raw=true)

Go to the section **Additional Configuration**  (expand it if necessary) and uncheck the option **Enable Backtrack**then click **Continue** at the bottom of the page.

![RDS Cluster Disable Backtrack](/images/aurora-rds-cluster-disable-backtrack.png?raw=true)

In the section **Scheduling of modifications**, choose the option **Apply immediately** and click **Modify Cluster** to confirm the changes.

![RDS Cluster Confirm Changes](/images/aurora-rds-cluster-modify-confirm.png?raw=true)

When the modification completes, and the DB cluster is in a state `available` again, on the drop-down button **Actions**, choose **Add region**..

{{% notice note%}}
You may need to refresh the browser page after disabling Backtrack before adding a region. If **Add region** appears unavailable, refresh the browser page, or make sure that Backtrack has been disabled correctly.
{{% /notice%}}

-->
On the drop-down button **Actions**, choose **Add region**.

![RDS Cluster Add Region](/images/aurora-rds-cluster-action-add.png?raw=true)

Set the following options on the configuration screen for the secondary DB cluster:

1.  In the section **Global database settings**:
    *   In **Global database identifier** put `auroralab-mysql-global`

2.  In the section **AWS Region**:
    *   Choose the **secondary region** from `US East (N. Virginia)`

3.  In **Connectivity**, specify where the DB cluster will be deployed with the network information created above:
    *   In **Virtual Private Cloud (VPC)** put `auroralab-vpc`
    *   Make sure the right  **Subnet Group** has been automatically selected, must have the name `auroralab-db-subnet-group`.
    *   Make sure the option **Publicly accessible** is set to `No`
    *   For **VPC security group** select **Choose existing** and choose the security group called `auroralab-database-sg`, remove any other security groups, such as `default` of the selection.

4.  In **Read replica write forwarding**:
    *   **Mark** the option **Enable read replica write forwarding**

5.  Expand the section **Advanced configuration**, and configure the following options:
    *   For **DB instance identifier** choice `auroralab-mysql-node-3`
    *   For **DB cluster identifier** choice `auroralab-mysql-secondary`
    *   For **DB cluster parameter group** select the group with the name of the stack in the name (e.g. `auroralab-[...]`)
    *   For **DB parameter group** select the group with the name of the stack in the name (e.g. `auroralab-[...]`)
    *   For **Backup retention period** choice `1 day`
    *   **Mark** the option **Enable Performance Insights**
    *   For **Retention period** choice `Default (7 days)`
    *   For **Master key** choice `[default] aws/rds`
    *   **Mark** the option **Enable Enhanced Monitoring**
    *   For **Granularity** choice `1 second`
    *   For **Monitoring Role** choice `auroralab-monitor-[secondary-region]`

{{% notice info%}}
Notice there is **two** monitoring functions in the list, one for the primary region (one in the upper right corner), the other for the secondary region (normally `us-east-1`). In this step, you need the region **secondary**.
{{% /notice%}}
![RDS Cluster Add Region](/images/aurora-rds-cluster-add-region.png?raw=true)

{{% notice note%}}
What do these options mean?

You will create a global cluster, a secondary DB cluster, and a DB instance in that secondary cluster, in a single step. Your existing DB cluster will become the primary DB cluster in the new global cluster. These are distinct API calls to the RDS service if you create a global cluster using the AWS CLI, SDKs, or other tools. The RDS service console simply combines these distinct steps into a single operation.
{{% /notice%}}

Click **Add region** to create the global cluster.

{{% notice note%}}
Creating a global cluster based on the existing DB cluster is a transtransparent operation, the current cluster will not be disrupted. You can monitor performance metrics throughout this operation.
{{% /notice%}}

The global cluster, including the secondary DB cluster, can take up to 30 minutes to provision.

To connect to the DB cluster and start using it, you must retrieve the database cluster access addresses. Unlike a regular DB cluster, only **Reader Endpoint** is provisioned. THE **Cluster Endpoint** is not created because secondary database clusters contain only readers and cannot accept writes. THE **Reader Endpoint** It will always resolve for one of the database instances and should be used for low-latency read operations in that region. In the RDS console, go to the DB cluster details view by clicking on **cluster detail** for the secondary DB cluster, called `auroralab-mysql-secondary`.

The section **Endpoints** on the tab **Connectivity and security** the details page displays the endpoints.

![RDS Cluster Secondary Endpoints](/images/aurora-rds-cluster-secondary-endpoints.png?raw=true)