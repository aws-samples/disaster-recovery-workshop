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

These exercises will show you the steps for using the Global Database engine for Amazon Aurora for MySQL databases. If you want more information [click here]({{< ref "/services/databases/aurora" >}} "Amazon Aurora").

**By the end of this exercise, you will be able to:**

- Create database clusters
- Monitor replication performance
- Perform cross-region failover operation

**Estimated Duration:** 2 hours

**Approximate Cost**: 20 USD

# Create an Aurora Global Database

Amazon Aurora <a href="https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-global-database.html" target="_blank">Global Database</a> is designed for globally distributed applications, allowing a single Amazon Aurora database to cover multiple AWS Regions. It replicates your data without impact on database performance, enables fast local reads with low latency in each region, and provides disaster recovery from outages in a region.

This lab contains the following tasks:

1.  Create a Lab Environment in the Ohio Region (us-east-2)
2.  Create a lab environment in the Virginia Region (us-east-1)
3.  Create an Aurora Global Cluster

# 1. Create a Lab Environment in the Ohio Region (us-east-2)

 <h4>Acesse o AWS Management Console </h4>

You can access the console at: <a href="https://console.aws.amazon.com/" target="_blank">https://console.aws.amazon.com/</a> or through the single sign-on (SSO) mechanism provided by your organization.

![AWS Management Console Login](/images/aurora-2-login.png?raw=true)

Labs are designed to work in any of the regions where Amazon Aurora compatible MySQL is available. However, not all Amazon Aurora features and capabilities may be available in all currently supported regions.

This lab uses the US East (N. Virginia) /us-east-1 region for the Aurora Global Database secondary region, in which case we recommend using the Ohio region /us-east-1 for the primary environment.

![Seleção de região do AWS Management Console](/images/aurora-2-region-select.png?raw=true)

<h4>Criar o ambiente de laboratório usando AWS CloudFormation </h4>

To simplify the initial lab experience, we created basic models for <a href="https://aws.amazon.com/cloudformation/" target="_blank"> AWS CloudFormation </a> that provision the resources required for the lab environment. These models are designed to deploy a consistent network infrastructure and components used in the lab.

Click  **Launch Stack to create an Aurora Provisioned DB Cluster Automatically**:

<a href="https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create/review?stackName=auroralab&templateURL=https://s3.amazonaws.com/ams-stack-prod-content-us-east-1/templates/lab_template.yml&param_deployCluster=Yes&param_deployGDB=Yes" target="_blank"><img src="https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png" alt="Launch Stack"></a>

In the field called **Stack Name**, make sure the value `auroralab` is default.

To run the Aurora Global Database Lab, also select **Yes** for the parameter **Enable Aurora Global Database Labs?**

![Create Stack](/images/aurora-2-create-stack-params.png?raw=true)

Go to the bottom of the page, check the box that says: **I acknowledge that AWS CloudFormation might create IAM resources with custom names** and click **Create stack**.

![Create Stack](/images/aurora-2-create-stack-confirm.png?raw=true)

The stack will take approximately 20 minutes to provision. You can monitor the status on the page **Stack detail**. You can monitor the progress of the stack creation process by updating the tab **Events**. The last event on the list will indicate `CREATE_COMPLETE` for the stack feature.

![Stack Status](/images/aurora-2-stack-status.png?raw=true)

When the stack status is `CREATE_COMPLETE`, click the tab **Outputs**. The values here will be essential for the completion of the rest of the laboratory. Take a moment to save these values in a location where you have easy access to them during the rest of the lab. The names that appear in the column **Key** are referenced directly in the instructions in subsequent steps, using the parameter format: == \[OutputKey] ==.

![Stack Outputs](/images/aurora-2-stack-outputs.png?raw=true)

Now let's check the access to Session Manager, from where we'll run some commands in the following labs.

* [Connect to the Session Manager Workstation ]({{% relref connect %}})

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

Open the <a href="https://console.aws.amazon.com/rds/home#database:id=auroralab-mysql-cluster;is-cluster=true" target="_blank">Amazon RDS service console</a> on the MySQL DB cluster details page in the region **primary**. If you navigated to the RDS console by means other than the link in this paragraph, click `auroralab-mysql-cluster` in the section **Databases** from the RDS service console, and make sure you are back in the primary region.

{{% notice note%}}
Make sure you're still working on **primary region**, especially if you're following the links above to open the service console on the right screen.
{{% /notice%}}

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

# Monitor the Global Database

Amazon Aurora Exposes a Variety of <a href="https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Monitoring.html" target="_blank"> Amazon CloudWatch metrics </a>, which you can use to monitor and determine the health and performance of your global Aurora database. In this lab, you'll create an Amazon CloudWatch dashboard to monitor latency, replicated IO, and cross-region replication data transfer to our global Aurora database.

This lab contains the following tasks:

1.  Generate load on the primary DB cluster
2.  Monitor Cluster Load and Replication Delay

This laboratory requires the following prerequisites:

*   [Connect to the Session Manager Workstation ]({{% relref connect %}})

## 1. Generate load on the primary DB cluster

You will use the benchmark script similar to Percona's TPCC based on sysbench to generate the load. To put it simply, we packaged the correct set of commands into a <a href = "https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-ssm-docs.html" target = "_ blank" > AWS Systems Manager Command Document </a>. You will use <a href="https://docs.aws.amazon.com/systems-manager/latest/userguide/execute-remote-commands.html" target="_blank"> AWS Systems Manager Run Command </a> to run the test.

If you're not already connected to the Session Manager terminal, sign in [following these instructions]({{% relref connect %}}) onto **primary region**. Once connected, enter the following commands, replacing the placeholders appropriately.

{{% notice warning%}}
Make sure you're still working on **primary region**
{{% /notice%}}

```shell
aws ssm send-command \
--document-name [loadTestRunDoc] \
--instance-ids [ec2Instance] 
```

{{% notice tip%}}
Don't forget to check if Session Manager is open in the region **primary**.
{{% /notice%}}

*   The value for **\[LoadteStrundoc]** is defined in the CloudFormation Stack output used to create the primary environment. Normally the amount will be **auroralab-sysbench-test**.
*   THE **\[EC2 Instance]** is the workspace instance ID, and it can be found at the top of the Session Manager screen. Or in the CloudFormation Stack output.

{{% notice info%}}
What do all these parameters mean?
Parameter | Description
— | —
—document-name | The name of the command document to run.
—instance-ids | The EC2 instance to run this command.
{{% /notice%}}

The command will be sent to the EC2 instance that will prepare the test dataset and run the load test. It can take up to a minute for CloudWatch to reflect the additional burden on metrics. You will see a confirmation that the command has started.

![SSM Command](/images/aurora-ssm-cmd-sysbench.png?raw=true)

## 2. Monitor Cluster Load and Replication Delay

On the RDS service console on **primary region**, select `auroralab-mysql-cluster` (primary), if not already selected and switch to tab **Monitoring**. You'll see a combined view of the write and read DB instances in that cluster. You are not using the read nodes at this time, the load is directed only to the write node. Browse metrics and analyze metrics specifically **CPU Utilization**, **Commit Throughput**, **DML Throughput**, **Select Throughput** and note that they are reasonably stable in addition to the initial peak caused by the sysbench tool by filling out an initial dataset.

![RDS Cluster Primary Metrics](/images/aurora-rds-cluster-primary-metrics.png?raw=true)

Then you will shift focus to **secondary DB cluster** newly created. You'll create a CloudWatch dashboard to monitor three key metrics relevant to global clusters and secondary database clusters, more specifically:

CloudWatch Metric Name | Description
— | —
`AuroraGlobalDBReplicatedWriteIO` | Write IO number replicated to the secondary region
`AuroraGlobalDBDataTransferBytes` | The amount of redo logs transferred to the secondary region, in bytes
`AuroraGlobalDBReplicationLag` | How ago, measured in milliseconds, the secondary region is behind the cluster in the primary region

Open the <a href="https://console.aws.amazon.com/cloudwatch/home?region=us-east-1#dashboards:" target="_blank">Amazon CloudWatch console</a> in the secondary region (us-east-1), in the menu  **Dashboards**.

{{% notice warning%}}
You will work in a different region in subsequent steps: N. Virginia (us-east-1). Because you can have multiple browser tabs and command-line sessions open, make sure you're always operating in the desired region.
{{% /notice%}}

![CloudWatch Dashboards Listing](/images/aurora-cw-dash-listing.png?raw=true)

Click **Create dashboard**. Set the name of the new dashboard `auroralab-gdb-dashboard` and click **Create dashboard**.

![CloudWatch Dashboard Creation](/images/aurora-cw-dash-create.png)

Let's add our first widget to the dashboard that will show our replication latency between the secondary and primary Aurora cluster. Select **Number** and click **Next**.

![CloudWatch Dashboard Add Number Widget](/images/aurora-cw-dash-add-number.png)

On the screen of **Add Metric Graph**, we will examine the tab **All Metrics**, we will select **RDS** and then we'll select the metric group called **Source Region**.

Now you should see a filtered metric `AuroraGlobalDBReplicationLag`, with the SourceRegion column as the name of your primary global cluster region. Select this metric using the check box.

The widget preview should now be at the top with a sample of the delay time in milliseconds. Let's update the widget further. Give it a friendly name by clicking the edit icon (pencil icon) and rename the widget `Untitled Graph` towards`Global DB Replication Lag (avg, 1min)`, press the markup icon to submit your changes.

At the bottom, click the **Graphed Metrics** to further customize our view. In the column **Statistic**, change to `Average` and **Period** towards`  1 minuto `.

Confirm that the settings are the same as the example below, and click **Create widget**.

![CloudWatch Widget Configuration](/images/aurora-cw-widget-setup.png)

Now you've created your first widget. You can set an automatic update at a range defined in the refresh menu in the upper right corner.

Click **Save dashboard** to save the changes.

![CloudWatch Dashboard Single Widget](/images/aurora-cw-dash-single-widget.png)

You can add widgets individually to the dashboard to build a more complete monitoring panel. However, to save time, let's use the JSON below.

First, click the drop-down list **Actions** on the dashboard and choose **View/Edit source**.

![CloudWatch Dashboard Actions](/images/aurora-cw-dash-actions.png)

In the text box that appears on the screen, paste the following JSON code. Be sure to update the AWS Region in the code below to match your **secondary region**if necessary. Also, if you used different DB cluster identifiers (names) for DB clusters than those indicated in this guide, you will also have to update them.

```json
{
    "widgets": [
        {
            "type": "metric",
            "x": 0,
            "y": 3,
            "width": 24,
            "height": 6,
            "properties": {
                "metrics": [
                    [ "AWS/RDS", "AuroraGlobalDBReplicationLag", "DBClusterIdentifier", "auroralab-mysql-secondary" ],
                    [ "...", { "stat": "Maximum" } ]
                ],
                "view": "timeSeries",
                "stacked": false,
                "region": "us-east-1",
                "title": "Global DB Replication Lag (max vs. avg, 1min)",
                "stat": "Average",
                "period": 60
            }
        },
        {
            "type": "metric",
            "x": 0,
            "y": 0,
            "width": 9,
            "height": 3,
            "properties": {
                "metrics": [
                    [ "AWS/RDS", "AuroraGlobalDBReplicationLag", "DBClusterIdentifier", "auroralab-mysql-secondary" ]
                ],
                "view": "singleValue",
                "region": "us-east-1",
                "title": "Global DB Replication Lag (avg, 1min)",
                "stat": "Average",
                "period": 60
            }
        },
        {
            "type": "metric",
            "x": 9,
            "y": 0,
            "width": 15,
            "height": 3,
            "properties": {
                "metrics": [
                    [ "AWS/RDS", "AuroraGlobalDBReplicatedWriteIO", "DBClusterIdentifier", "auroralab-mysql-secondary", { "label": "Global DB Replicated Write IOs" } ],
                    [ ".", "AuroraGlobalDBDataTransferBytes", ".", ".", { "label": "Global DB DataTransfer Bytes" } ]
                ],
                "view": "singleValue",
                "region": "us-east-1",
                "stat": "Sum",
                "period": 86400,
                "title": "Billable Replication Metrics (aggregate, last 24 hr)"
            }
        }
    ]
}
```

Click **Update** to change the dashboard.

![CloudWatch Dashboard Source](/images/aurora-cw-dash-source.png)

Click **Save dashboard** to ensure that the dashboard is saved with the new changes.

![CloudWatch Dashboard Multiple Widgets](/images/aurora-cw-dash-multi-widget.png)

{{% notice note%}}
The metrics **Aurora Global DB Replicated I/O** and **Aurora Global DB Data Transfer Bytes**  are only updated once an hour, so you may not be able to see the data for those metrics depending on the time of your lab.
{{% /notice%}}

# Fail Over at Aurora Global Database

When used in combination with replicas in the region, an Aurora DB cluster provides automatic failover capabilities within the region. With Aurora Global Database, you can perform a manual failover to the DB cluster in your secondary region so that your database can survive in the unlikely scenario of an entire region infrastructure or service becoming unavailable.

Combined with an application layer implemented across multiple regions (via immutable infrastructure or (<a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/CopyingAMIs.html" target="_blank"> copying your cross-region AMIs </a>), you can further increase the availability of your applications while maintaining data consistency.

This lab contains the following tasks:

1. Inject a fault into the primary region
2. Promote the secondary DB cluster

This laboratory requires the following prerequisites:

* [Connect to the Session Manager Workstation ]({{% relref connect %}})
* Aurora Global Database Cluster Created and Working


## 1. Inject a fault into the primary region

If you're not already connected to the Session Manager workstation command line, connect [following these instructions]({{% relref connect %}}) onto **primary region**. Once connected, type one of the following commands, replacing the placeholders appropriately.

*   [Connect to the Session Manager Workstation ]({{% relref connect %}})

{{% notice warning%}}
Because you can have multiple browser tabs open, make sure you're always operating in the desired region.
{{% /notice%}}

```shell
mysql -h[clusterEndpoint] -u$DBUSER -p"$DBPASS" mylab
```

{{% notice tip%}}
Where can I find the cluster endpoint (or any other parameter)?

Because you're working with a global Aurora database, there are several endpoints in the primary and secondary regions that you'll have to keep track of. Check the end of the lab \[Deploy a Global Database] for instructions on how to find the cluster connection addresses.
{{% /notice%}}

Once connected to the database, use the code below to create a table. Run the following SQL queries:

```sql
DROP TABLE IF EXISTS failovertest1;
CREATE TABLE failovertest1 (
  pk INT NOT NULL AUTO_INCREMENT,
  gen_number INT NOT NULL,
  some_text VARCHAR(100),
  input_dt DATETIME,
  PRIMARY KEY (pk)
);
INSERT INTO failovertest1 (gen_number, some_text, input_dt)
VALUES (100,"region-1-input",now());
COMMIT;
SELECT * FROM failovertest1;
```

Take note of the query results, you'll use them later to compare.

You can simulate a region failure using multiple engines. However, in this case, you will simulate a long-term, large-scale (albeit infrequent) failure, and the best way to do this is to stop all input/outbound data traffic inside and outside the Aurora Global Database primary DB cluster. Your lab environment contains a <a href="https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html" target="_blank"> VPC ACL Network </a> (NACL) with a rule **DENY ALL** specific, which will block all input/outbound traffic from the associated subnets. This will basically emulate a wider flaw that will make the primary region DB cluster unavailable to your applications.

Open the <a href="https://console.aws.amazon.com/vpc/home#acls:sort=networkAclId" target="_blank"> VPC service console </a> onto **primary region** in the section **Network ACLs**. You should see a NACL called **auroralab-denyall-nacl** and that it's not currently associated with any subnet. Select it by checking the box next to the name and review the tabs **Inbound Rules** and **Outbound Rules**  in the bottom detail panel. You should see that they are defined as *DENY* for ALL traffic.

{{% notice warning%}}
Make sure you're still working on **primary region**, especially if you're following the links above to open the service console on the right screen.
{{% /notice%}}

![NACLs Rules Review](/images/aurora-vpc-nacl-rules.png)

Switch to tab **Subnet associations** from the selected NACL in the details pane and click the **Edit subnet associations**.

The Aurora DB cluster is configured to use **private subnets** configured for your lab environment and managed by the RDS DB subnet group created for your lab environment. All of these subnets are named with the prefix `auroralab-prv-sub-`, followed by a number corresponding to the Availability Zone. You may need to enlarge the column **Subnet ID**  to see the names.

Select all subnets that start with this prefix `auroralab-prv-sub-`. You can also simply use the search box to filter on any subnet with the name \*\* prv \*\* and select it. Then click the button **Edit** to confirm memberships.

![NACLs Edit Subnet Association](/images/aurora-vpc-subassoc-edit.png)

Once associated, the NACL rules take effect immediately and will make resources within private subnets inaccessible. Your connection to the database on the Session Manager command line should eventually be interrupted with an error message indicating that the client has lost its connection to the server.

## 2. Promote the secondary DB cluster

You are simulating an extended regional service level or infrastructure failure; at this point, you would normally choose to perform a regional failover on your DR site for your database applications and stacks. Then you will promote **secondary DB cluster**, on **secondary region** to a standalone regional DB cluster.

Open the <a href = "https://console.aws.amazon.com/rds/home?region=us-east-1#database:id=auroralab-mysql-secondary;is-cluster=true" target = " _blank "> Amazon RDS service console </a> on the MySQL DB cluster details page **secondary DB cluster**.

{{% notice warning%}}
Make sure you're working on **secondary region**, especially if you're following the links above to open the service console on the right screen.
{{% /notice%}}

{{% notice info%}}
Why does the status of my primary DB cluster and DB instance still appear as <i> Available </i>?

You may also notice that on your RDS console it will still show the primary region DB cluster and the DB instance as **Available**, that's because you're just simulating a flaw blocking all network access through NACLs and such a simulation is limited only to your AWS account, specifically your VPC and its affected subnets; the RDS/Aurora service and its internal health checks are provided by the service itself and still report the DB cluster and DB instance as healthy because there is none *actual interruption*.
{{% /notice%}}

With the secondary DB cluster `auroralab-mysql-secondary` selected, click the menu **Actions** and select **Remove from Global**.

![RDS Remove Cluster From Global](/images/aurora-rds-cluster-action-remglobal.png)

A message will appear prompting you to confirm. This action will stop replication of the primary DB cluster. Confirm by clicking **Remove and promote**.

![RDS Confirm Remove Cluster From Global](/images/aurora-rds-cluster-confirm-remglobal.png)

The promotion process should take less than 1 minute. Once it's complete, you can see that the previously secondary DB cluster is now labeled **Regional** and the DB instance is now a node of **Writer**.

Click the newly promoted DB cluster. On the tab **Connectivity and security**, the endpoint of **Writer** should now be listed as **Available**. Copy and paste the terminal string into your notepad.

![RDS Cluster Endpoints Promoted](/images/aurora-rds-cluster-endpoints-promoted.png)

Open an additional session in Session Manager in **secondary region**. See [Connect to Session Manager]({{% relref connect %}}), to get the step-by-step how to create a Session Manager command line session, but make sure to use **secondary region**.

{{% notice warning%}}
Make sure you're working on **secondary region**, especially if you're following the links above to open the service console on the right screen.
{{% /notice%}}

Once logged in, you need to configure the database credentials on the EC2 workstation in the secondary region. If you created the original primary DB cluster manually, you performed a similar step at that time. Run the following commands, replacing placeholders with values as indicated in the table below:

Placeholder | Where to Find
— | —
\== \[SecreTarn] == | You'll find it on the CloudFormation Stack outputs you used to provision the lab environment. The value starts with `arn:aws:secretsmanager:`
\== \[PrimaryRegion] == | The identifier of **primary region** that you're using, click the name in the upper right corner of the console. You'll find it next to the name, for example `us-east-2`, although your region may vary.

```shell
CREDS=`aws secretsmanager get-secret-value --secret-id [secretArn] --region [primaryRegion] | jq -r '.SecretString'`
export DBUSER="`echo $CREDS | jq -r '.username'`"
export DBPASS="`echo $CREDS | jq -r '.password'`"
echo "export DBPASS=\"$DBPASS\"" >> /home/ubuntu/.bashrc
echo "export DBUSER=$DBUSER" >> /home/ubuntu/.bashrc
```

Then run the command below, replacing the marker **\[NewCluster EndPont]** by the connection address of the newly promoted DB cluster.

```shell
mysql -h[newClusterEndpoint] -u$DBUSER -p"$DBPASS" mylab
```

Once connected to the database, use the code below to create a table. Run the following SQL queries:

```sql
SELECT * FROM failovertest1;
```

Note the query results, this should return the values you entered into the Database shortly before the simulated failure.

Enter a new record in this table. Copy and paste the following query and run it. What do you expect the results to be?

```sql
INSERT INTO failovertest1 (gen_number, some_text, input_dt)
VALUES (200,"region-2-input",now());
SELECT * FROM mylab.failovertest1;
```

Your application can now serve read and write queries where it used to be yours **secondary region** and serve your users normally during an outage across the region!

### Removing Labs

To remove the created lab environments, go to CloudFormation in each chosen region, select the stack used in the labs and choose the option **Delete** and then confirm by clicking **Delete stack**.

![CloudFormation](/images/aurora-cf-delete.png?width=50pc)

### Conclusion

With this exercise, it was possible to verify how to create an Aurora Global Cluster, monitor the update of cross-region data, and failover across regions.

**Source:** [Amazon Aurora Labs for MySQL](https://awsauroralabsmy.com/global/deploy/)

<!-- TODO: Incluir estimativa de custos!!! --->

