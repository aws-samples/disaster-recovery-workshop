---
hidden: true
type: lab
---


#### Go to AWS Cloud9

1. Create AWS Cloud9 environment in Primary Region **N. Virginia**

   More instructions [click here]({{% relref workspace %}}).

2. Install Redis CLI using Cloud9 Terminal
   
   In order to execute Redis commands, install Redis CLI in your environment.
    ```bash
     sudo yum install jq -y
     sudo amazon-linux-extras install epel -y
     sudo yum install gcc jemalloc-devel openssl-devel tcl tcl-devel -y
     sudo wget http://download.redis.io/redis-stable.tar.gz
     sudo tar xvzf redis-stable.tar.gz
     cd redis-stable
     sudo make BUILD_TLS=yes
    ```

3. Repeat steps 1 and 2 using the Secondary Region **N. California**

   {{% notice warning %}}
   *You'll need the two Cloud9 environments in order to access ElastiCache nodes from different regions.*
   {{% /notice %}}
#### Create an Amazon ElastiCache for Redis cluster

1. Using AWS Cloud9 terminal in **N. Virginia**, create a new Redis Cluster (**myCluster**) in the Primary Region.
    ```bash
     aws elasticache create-replication-group  --replication-group-id cluster-lab --replication-group-description "DR Workshop Labs" --engine redis --cache-node-type cache.r6g.large --region us-east-1
    ```
    
   {{% notice note %}}
   *Please wait about 5 minutes to cluster be available*
   {{% /notice %}}
    

2. Check the cluster status
   ```bash
    aws elasticache describe-replication-groups --replication-group-id cluster-lab --region us-east-1 | jq -r .ReplicationGroups[0].Status
   ```

3. Copy environment variables
    ```bash
     export PRIMARY_ENDPOINT=$(aws elasticache describe-replication-groups --replication-group-id cluster-lab | jq -r '.ReplicationGroups[0].NodeGroups[0].PrimaryEndpoint.Address')
     export PORT_NUMBER=$(aws elasticache describe-replication-groups --replication-group-id cluster-lab | jq -r '.ReplicationGroups[0].NodeGroups[0].PrimaryEndpoint.Port')
     echo $PRIMARY_ENDPOINT $PORT_NUMBER
    ```

#### Store keys on ElastiCache Cluster

1. Connect to Cluster
   ```bash
    src/redis-cli -h $PRIMARY_ENDPOINT -p $PORT_NUMBER
   ```

2. Test the connection
   ```bash
    PING
   ```

3. Write keys/value pairs
   
   With the connection opened perform the REDIS command below:
   ```bash
    SET counter 100
   ```
   ```bash
    INCR counter
   ```

   ```bash
    INCRBY counter 10
   ```

   ```bash
    SET userid 5000 
   ```

   ```bash
    KEYS * 
   ```  

   ```bash
    GET counter 
   ```      
   ```bash
    GET userid 
   ```

   
#### Create a Global Datastore from Regional Cluster

1.  Create  **US West (Oregon)**.
    ```bash
   
    ```

2.  (Optional) You can view the list of replicas created using ...
    ```bash
    
    ```

{{% notice note%}}
*If any of ... are not in the list of replicates, wait a few minutes for them to be created.*
{{% /notice%}}

#### Promote Secondary Region

1.  To verify that replication is working, add a new item to the Music table in the region **US East (Ohio)**.
    ```bash
    aws ...
        --region us-east-2
    ```

2.  To verify that replication is working, add a new item to the Music table in the region **US East (Ohio)**.
    ```bash
    aws ...
        --region us-west-2
    ```

3.  Make sure the items have been successfully replicated to the region **Europe (Ireland)**:
    ```bash
    aws ... --region eu-west-1
    ```

#### Cleaning up

1.  Delete the replica table in the region **Europe (Ireland)** and region **US West (Oregon)**.

    ```bash
    aws ...
    ```

2.  Wait a few minutes until both are deleted. You can check the deletion already finished using the command below:
    ```bash
    aws ...
    ```

3.  Delete ... in the region **US East (Ohio)**.
    ```bash
    aws ... --region us-east-2
    ```
