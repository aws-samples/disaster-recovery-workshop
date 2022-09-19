---
title: "Amazon Aurora - Global Database"
menutitle: "Fail Over at Aurora Global Database"
chapter: false
weight: 0.2
pre: "<b></b>"
tags: [Active-Active]
type: Lab
awsServices:
 - Amazon Aurora
---

# Failover at Aurora Global Database

When used in combination with replicas located at the same region, an **Aurora Database Cluster** (not global) can perform an automatic failover inside that region. If you are using the **Aurora Global Database** cluster, you can also perform a **manual failover** to your secondary region.
When combined with an application layer implementation across multiple regions (via immutable infrastructure or (<a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/CopyingAMIs.html" target="_blank"> copying your AMIs to the secondary region, for example</a>), you will be able to meet your RPO, RTO and SLA in the unlikely scenario of an entire region becoming unavailable.

Let's work now with an example on how these failover capabilities can be implemented on your environment. To demonstrate this, we will work with two main tasks:

1. Simulating a failure on your Primary Region
2. Promoting the Secondary Aurora Database Cluster to Primary

Before proceeding with these tasks, please make sure that the following prerequisites are already in place:

* You have a working Session Manager Workstation configured [Connect to the Session Manager Workstation ]({{% relref connect %}})

* You already have an Aurora Global Database Cluster created and working as expected.

## 1. Simulating a failure on your the Primary Region

If you're not already connected to the Session Manager Workstation, connect to it [following these instructions]({{% relref connect %}}) on your  **Primary Region**. Once connected, type one of the following commands, replacing the placeholders appropriately?

```shell
mysql -h[clusterEndpoint] -u$DBUSER -p"$DBPASS" mylab
```
{{% notice warning%}}
Because you can have multiple browser tabs open, make sure you're always operating in the desired region.
{{% /notice%}}

{{% notice tip%}}
Where can I find the cluster endpoint (or any other parameter)?

Because you're working with a **Aurora Global Database**, there are several endpoints in the primary and secondary regions that you'll have to keep track of. Check the end of the lab \[Deploy a Global Database] for instructions on how to find the cluster connection addresses.
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

You can simulate a region failure using multiple engines. However, in this case, you will simulate a long-term, large-scale (albeit infrequent) failure, and the best way to do this is to stop all input/outbound network traffic (incoming and outgoing) to the **Aurora Global Database Primary Database Cluster** node.

Your lab environment contains a <a href="https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html" target="_blank"> VPC Network ACL</a> (NACL) with a **DENY ALL** rule which will block all inbound and outbound traffic from the subnetst tha are assigned to it. This will  emulate a network flaw that will make the **Primary Region Database Cluster** unavailable to your applications.

To perform this network failure simulation, please open the <a href="https://console.aws.amazon.com/vpc/home#acls:sort=networkAclId" target="_blank"> VPC service console </a> in the **Primary Region** and them navigate to the **Network ACLs** section.
You should see a NACL called **auroralab-denyall-nacl**. Notice however that this ACL is currently not associated to any of your VPC Subnets. Review it by checking the box next it's name and check the **Inbound Rules** and **Outbound Rules** tabs at the detail panel at the bottom. You should see that these rules are defined as *DENY* for ALL traffic.

{{% notice warning%}}
Make sure you're still working in the **Primary Region**, especially if you're following the links above to open the service console.
{{% /notice%}}

![NACLs Rules Review](/images/aurora-vpc-nacl-rules.png)

Switch now to the **Subnet associations** tab in the details pane and click on **Edit subnet associations**.

The **Aurora Database Cluster** is configured to use **private subnets** that were configured for your lab environment .These subnets are named with the `auroralab-prv-sub-` prefix, followed by a number corresponding to the Availability Zone in which they are placed. You may need to enlarge the column **Subnet ID**  to see their full name.

To proceed, select all subnets that start with this prefix `auroralab-prv-sub-`. You can also simply use the search box to filter on any subnet with the name `prv` and select them. Click at **Edit** button to confirm the association of the NACLs to these Subnets.

![NACLs Edit Subnet Association](/images/aurora-vpc-subassoc-edit.png)

Once configured, these NACL rules will take effect immediately and will make the resources within private subnets inaccessible from the network perspective. Your connection to the database on the **Session Manager Command Line** should eventually be interrupted with an error message indicating that the client has lost connection to the server.

## 2. Promoting the Secondary Database Cluster to Primary

{{% notice info%}}
In a real world scenario, you would need to address the failover of your applications and other infrastructure elements (such as EC2, Fargate containers, etc) alongside with the Database Failover. Since this is an exercise that is limited the Database aspect of a Disaster Recovery event, we will not be covering all these steps..  
{{% /notice%}}

Let's now open the <a href = "https://console.aws.amazon.com/rds/home?region=us-east-1#database:id=auroralab-mysql-secondary;is-cluster=true" target = " _blank "> Amazon RDS console </a> at the MySQL Database Cluster details page at the **Secondary Database Cluster**.

{{% notice warning%}}
Make sure you're working in the **Secondary Region**, especially if you're following the links above to open the console.
{{% /notice%}}

{{% notice info%}}
Why does the status of my **Primary Database Cluster** and my Database instance still appears as <i> Available </i>?

You may have noticed that on your RDS console id still showing the **Primary Region Database Cluster** and the **Database Instance** as **Available**. Since we are just simulating a disaster event by leveraging NACLs to block all network traffic,this failure simulation is running at the network scope (your VPC and the cluster subnets) only. Therefore,the RDS Aurora Service itself is still running as expected and the internal health checks are still reporting as **healthy** since there is no actual **database failure** at the moment.
{{% /notice%}}

With the **Secondary Cluster** `auroralab-mysql-secondary` selected, click the menu **Actions** and select **Remove from global database**.

![RDS Remove Cluster From Global Database](/images/aurora-rds-cluster-action-remglobal.png)

A message will appear prompting you to confirm. This action will stop replication of the **Primary  Cluster**. Confirm by clicking **Remove and promote**.

![RDS Confirm Remove Cluster From Global](/images/aurora-rds-cluster-confirm-remglobal.png)

The promotion process should take less than 1 minute. Once it's complete, you can see that the previously secondary DB cluster is now labeled **Regional** and the Database instance Role changed to **Writer**.

Click now at the previously Secondary - and now Primary - cluster. At the tab **Connectivity and Security**, the **Writer** endpoint should now be listed as **Available**. To connect to this endpoint, copy and paste the **Endpoint name** terminal string into your notepad.

![RDS Cluster Endpoints Promoted](/images/aurora-rds-cluster-endpoints-promoted.png)

Open now an additional session in Session Manager in the **Secondary Region** (details at [Connect to Session Manager]({{% relref connect %}})).

{{% notice warning%}}
Make sure that you're working on **secondary region**, especially if you're following the links above to open the service console.
{{% /notice%}}

Once logged in, change to the `ec2-user`.

```shell
sudo su -l ec2-user
```

Now, you will need to configure the database credentials on the EC2 workstation in the secondary region. Run the following commands, replacing the `SecretArn` with the  CloudFormation Stack outputs you used to provision the lab environment. The value starts with `arn:aws:secretsmanager:`.

```shell
CREDS=`aws secretsmanager get-secret-value --secret-id [secretArn] --region us-east-1 | jq -r '.SecretString'`
export DBUSER="`echo $CREDS | jq -r '.username'`"
export DBPASS="`echo $CREDS | jq -r '.password'`"
echo "export DBPASS=\"$DBPASS\"" >> /home/ubuntu/.bashrc
echo "export DBUSER=$DBUSER" >> /home/ubuntu/.bashrc
```

Run now the command below, replacing the placeholder **\[NewCluster EndPoint]** with the Endpoint Address of the newly promoted DB cluster that you have few steps before:

```shell
mysql -h[newClusterEndpoint] -u$DBUSER -p"$DBPASS" mylab
```

Once connected to the database, let's query the database items using the **Select SQL** command below:

```sql
SELECT * FROM failovertest1;
```

Let's create now a new entry to this table. Copy and paste the following SQL statements and execut them:

```sql
INSERT INTO failovertest1 (gen_number, some_text, input_dt)
VALUES (200,"region-2-input",now());
SELECT * FROM mylab.failovertest1;
```

Congratulations!
As you can see, your database can now serve read and write requests at your **Secondary Region**. Your users and applications can now work normally during an outage that affected the **Primary Region**.

### Removing the resources created by the Labs

To remove all the resources created during the execution of our lab exercises, let's go to the CloudFormation console at both regions (us-east-1 and us-west-1). Select now the stacks that were used to create our labs and remove them by clicking at the **Delete** button and confirm the removal by selecting **Delete stack**.

![CloudFormation](/images/aurora-cf-delete.png?width=50pc)

### Conclusion

With this exercise, it was possible to verify how to create an Aurora Global Cluster, monitor the update of cross-region data, and failover across regions.

**Source:** [Amazon Aurora Labs for MySQL](https://awsauroralabsmy.com/global/deploy/)

<!-- TODO: Incluir estimativa de custos!!! --->
