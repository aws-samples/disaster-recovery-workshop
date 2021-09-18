---
hidden: true
---


#### Go to AWS CloudShell

1. Go to AWS CloudShell, in the top bar of the AWS Console, click the button on the right side of the search bar.

<img src="/images/console-cloudshell2.png?classes=shadow" />

#### Create an Amazon DynamoDB table

1. Create a new table **Music** in the region **US East (N. Virginia)**.
    ```bash
        aws dynamodb create-table \
            --table-name Music \
            --attribute-definitions \
                AttributeName=Artist,AttributeType=S \
                AttributeName=Song,AttributeType=S \
            --key-schema \
                AttributeName=Artist,KeyType=HASH \
                AttributeName=Song,KeyType=RANGE \
            --billing-mode PAY_PER_REQUEST \
            --stream-specification StreamEnabled=true,StreamViewType=NEW_AND_OLD_IMAGES \
            --region us-east-1
    ```

#### Create a replica of the table in another AWS Region

1. Create a table **Music** identical in **US West (N. California)**.
    ```bash
    aws dynamodb update-table --table-name Music --cli-input-json --region us-east-1 \
        '{
            "ReplicaUpdates":
            [
                {
                "Create": {
                    "RegionName": "us-west-1"
                    }
                }
            ]
        }' 
    ```

2. (Optional) You can view the list of replicas created using describe-table.
    ```bash
    aws dynamodb describe-table --table-name Music --region us-east-1
    ```

    {{% notice note%}}
   *If any of the tables are not in the list of replicates, wait a few minutes for them to be created.*
   {{% /notice%}}

#### Enter sample data

1.  To verify that replication is working, add a new item to the Music table in the region **US East (N. Virginia)**.
    ```bash
    aws dynamodb put-item \
        --table-name Music \
        --item '{"Artist": {"S":"The Beatles"},"Song": {"S":"Hey Jude"}}' \
        --region us-east-1
    ```

2.  To verify that replication is bidirectional, add a new item to the Music table in the region **US East (N. California)**.
    ```bash
    aws dynamodb put-item \
        --table-name Music \
        --item '{"Artist": {"S":"The Clash"},"Song": {"S":"London Calling"}}' \
        --region us-west-1
    ```

3.  Make sure the items have been successfully replicated to the regions:
    ```bash
    aws dynamodb scan --table-name Music --region us-east-1
    aws dynamodb scan --table-name Music --region us-west-1
    ```

#### Cleaning up

1.  Delete the replica table in the region **US West (N. California)**.

    ```bash
    aws dynamodb update-table --table-name Music --cli-input-json --region us-east-1 \
    '{
        "ReplicaUpdates":
        [
            {
            "Delete": {
                "RegionName": "us-west-1"
                }
            }
        ]
    }'
    ```

2.  Wait a few minutes until the secondary table be deleted. The primary table status will change from *UPDATING* to *ACTIVE*. You can check the deletion already finished using the command below:
    ```bash
    aws dynamodb describe-table --table-name Music --region us-east-1 | jq -r '.Table.TableStatus'
    ```

3.  After the deletion of secondary table be completed, delete the primary table **Music** in the region **US East (N. Virginia)**.
    ```bash
    aws dynamodb delete-table --table-name Music --region us-east-1
    ```
