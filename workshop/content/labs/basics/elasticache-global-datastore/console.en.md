---
hidden: true
type: lab
draft: true
---


#### Create an Amazon DynamoDB table

1.  Go to the AWS DynamoDB console: <https://console.aws.amazon.com/dynamodb>. <br>For this example, choose the region **us-east-2 (US East (Ohio))**.
2.  In the navigation pane, on the left side of the console, select **Tables (Tables)**.
3.  Choose **Create Table (Create Table)**. <br>In **Table name (Table name)**, insert **Music**. <br>In **Partition key (Primary Partition)** insert **Artist**. In **Sort key (Sorting key)** and enter **Song**. (Artist and Song must be strings). ![Create Table](/images/dynamodb-create-table.png?classes=shadow")
4.  To create the table, select **Create table (Create table)**. This table serves as the first replica table in a new global table. It is the prototype of other replica tables that will be created later.

#### Create a replica of the table in another AWS Region

1.  After creating the table in the previous step, select the table **Music**. Go to the tab **Global Tables (Global Tables)** and select **Create replica (Create Replica)**.
2.  In **Available replication Regions (Available Replication Regions)**, select **US West (Oregon)**. ![Create Replica](/images/dynamodb-create-replica-oregon.png?classes=shadow") <br>Select **Create Replica (Create Replica)**, this starts the process of creating the table at **US West (Oregon)**. <br>The tab **Global Table (Global Table)** from the selected table (and any other replica table) shows that the table has been replicated and its regions.\ <br>The console checks that there are tables with the same name in other regions. If there is a table with the same name, you must delete the existing table to create another replica table in that region.
    ![Global Tables List](/images/dynamodb-global-tables-list.png?classes=shadow")

#### Enter sample data

1.  You should still be using the AWS Management Console in the Region **us-east-2 (US East (Ohio))**. To the table **Music**, select the tab **Items** and choose **Create Item (Create Item)**. In **Artist**, insert **John Lennon**. In Song, enter Song **Imagine**. <br>To save the item, choose **Create item (Create Item)**. ![Create Item](/images/dynamodb-create-item-1.png?classes=shadow")
2.  The item will be replicated to other regions with replica tables. To confirm this, in the region selector in the upper right corner of the console, select **US West (Oregon)**.
    Select the table **Music** and in the items tab make sure that the created item has been replicated.
    ![Item replicated](/images/dynamodb-item-replicated.png?classes=shadow")

#### (Optional) Create more items through the replica table

1.  You should still be using the AWS Management Console in the Replica Region **(US West (Oregon))**. To the table **Music**, select the tab **Items** and choose **Create Item (Create Item)**. On the right side, select the input option **JSON**, copy the contents of the frame below.

2.  In **Artist**, insert **U2**. In Song, enter Song **One**. <br>To save the item, choose **Create item (Create Item)**. ![Create Item](/images/dynamodb-create-item-2.png?classes=shadow")

The items are replicated to all replica regions, regardless of which region the item was entered.

#### Cleaning up

1.  In the AWS Management Console in the Region **(US West (Oregon))**. To the table **Music**, select the table and click **Delete (Delete)**
    Confirm by typing **Delete** and then selecting **Delete table (delete table)**.
2.  Repeat the previous step for the region **us-east-2 (US East (Ohio))**.
