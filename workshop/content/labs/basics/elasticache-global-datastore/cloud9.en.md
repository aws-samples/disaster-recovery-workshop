---
hidden: true
type: lab
---


#### Go to AWS Cloud9

1. Create AWS Cloud9 environment in Primary Region **N. Virginia**

   For more instructions [click here]({{% relref workspace %}}).

2. Install Redis CLI using Cloud9 Terminal
   
   In order to execute Redis commands, install Redis CLI in your Cloud9 environment.
    ```bash
     # Install command-line JSON processor
     sudo yum install jq -y
     # Install dependencies
     sudo amazon-linux-extras install epel -y
     sudo yum install gcc jemalloc-devel openssl-devel tcl tcl-devel -y
     # Download the redis source code
     sudo wget http://download.redis.io/redis-stable.tar.gz
     sudo tar xvzf redis-stable.tar.gz
     cd redis-stable
     # Build Redis components
     sudo make BUILD_TLS=yes
    ```

3. Repeat steps 1 and 2 using the Secondary Region **N. California**

   {{% notice warning %}}
   *You'll need the two Cloud9 environments in order to access ElastiCache nodes from different regions.*
   {{% /notice %}}

#### Create an Amazon ElastiCache for Redis cluster

1. Using AWS Cloud9 terminal in **N. Virginia**, create a new Redis Cluster (**cluster-primary**) in the Primary Region.
    ```bash
    aws elasticache create-replication-group \
     --replication-group-id cluster-primary \
     --replication-group-description "DR Workshop Labs" \
     --engine redis \
     --multi-az-enabled \
     --cache-node-type cache.r6g.large \
     --num-cache-clusters 2 \
     --region us-east-1
    ```
    
   {{% notice note %}}
   *Please wait about 5 minutes to cluster be available.*
   {{% /notice %}}
    

2. Check the cluster status
   ```bash
   aws elasticache describe-replication-groups \
    --replication-group-id cluster-primary \
    --region us-east-1 |\
    jq -r .ReplicationGroups[0].Status
   ```

3. Copy environment variables
    ```bash
    # Export variables
    export PRIMARY_ENDPOINT=$(aws elasticache describe-replication-groups --replication-group-id cluster-primary | jq -r '.ReplicationGroups[0].NodeGroups[0].PrimaryEndpoint.Address')
    export PORT_NUMBER=$(aws elasticache describe-replication-groups --replication-group-id cluster-primary | jq -r '.ReplicationGroups[0].NodeGroups[0].PrimaryEndpoint.Port')
    # Show variables values
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

3. Write key-value pairs
   
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

   ```bash
    QUIT
   ```
   
#### Create a Global Datastore from Regional Cluster

1.  Create a Global Datastore using the primary replication group.
    ```bash
    aws elasticache create-global-replication-group \
     --global-replication-group-id-suffix multi-region \
     --primary-replication-group-id cluster-primary \
     --region us-east-1
    ```

    {{% notice note %}}
   *Please wait about 5 minutes to cluster be available.*
   {{% /notice %}}
    
2. Create new cluster in the Secondary Region **N. California** and add to Global Datastore
    ```bash
     aws elasticache create-replication-group \
     --replication-group-id cluster-secondary \
     --replication-group-description "DR Workshop Labs" \
     --global-replication-group-id ldgnf-multi-region \
     --multi-az-enabled \
     --num-cache-clusters 2 \
     --region us-west-1
    ```
    
   {{% notice note %}}
   *Each suffix identifies one region. The suffix **"ldgnf"** identifies the region **N. Virginia** and guarantees uniqueness of the global datastore name across multiple regions. [See documentation for more details](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Redis-Global-Datastores-CLI.html).*
   {{% /notice %}}


3. Check if both clusters are with Status "associated"
   ```bash
    aws elasticache describe-global-replication-groups \
     --global-replication-group-id ldgnf-multi-region \
     --show-member-info --region us-east-1 |\
     jq -r .GlobalReplicationGroups[0].Members
   ```

      {{% notice note %}}
   *Please wait about 10 minutes to new cluster be associated to Global Datastore.*
   {{% /notice %}}

#### Test the Secondary Region cluster
1. Using the AWS Cloud9 in the secondary region **N. California**
    ```bash
    # Export variables
    export PRIMARY_ENDPOINT=$(aws elasticache describe-replication-groups --replication-group-id cluster-secondary --region us-west-1 | jq -r '.ReplicationGroups[0].NodeGroups[0].PrimaryEndpoint.Address')
    export PORT_NUMBER=$(aws elasticache describe-replication-groups --replication-group-id cluster-secondary --region us-west-1 | jq -r '.ReplicationGroups[0].NodeGroups[0].PrimaryEndpoint.Port')
    # Show variables values
    echo $PRIMARY_ENDPOINT $PORT_NUMBER

2. Connect to Secondary Cluster
   ```bash
   src/redis-cli -h $PRIMARY_ENDPOINT -p $PORT_NUMBER
   ```

3. Write key-value pairs
   
   With the connection opened perform the REDIS command below:
   ```bash
    KEYS *
   ```

   ```bash
    GET counter
   ```

   ```bash
    GET userid
   ```

4. Try to create and modify the keys

   ```bash
    SET new-key somevalue
   ```

   The message **error) READONLY You can't write against a read only replica** will be shown, because it's not permitted to execute write operation on READONLY instances.

   You can check the instance operation mode by ROLE attribute using the command below:

   ```bash
    INFO replication
   ```  

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
