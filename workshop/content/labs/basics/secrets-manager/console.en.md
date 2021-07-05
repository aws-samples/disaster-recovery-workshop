---
title: "AWS Secrets Manager - Secrets Replication"
menutitle: "AWS Secrets Manager - Secrets Replication"
weight: 1
pre: "<b></b>"
hidden: true
---

#### Create a Secret

1.  Access the AWS Secrets Manager service: [https://console.aws.amazon.com/secretsmanager/](https://console.aws.amazon.com/secretsmanager/home?region=us-east-1)
2.  Click **Store a new secret**
3.  There are screen **Store a new secret**, select **Other type of secrets**. This type of secret is used when it doesn't apply to a database.
4.  In the section **Specify the key/value pairs to be stored in this secret**, in the first field type **MyFirstSecret**, enter your secret in the second field.![Secret - Create new secret](/images/secretmanager-create.png)
5.  Click **Next**
6.  Enter the name of the secret in the field **Secret name**
7.  Click **Next**
8.  In step 3, click **Next**
9.  In step 4 - **Review**, click **Next**

#### Replicating the Secret

1.  Locate the Secret Created in **AWS Secrets Manager > Secrets**
    ![looking](/images/secretmanager-looking.png)

2.  Click the Secret you want to replicate to another region

3.  On the Selected Secret tab, click **Replicate secret to other regions**
    ![accessing](/images/secretmanager-accessing.png)

4.  On the tab **Add replica regions**, configure replication details. In the field **AWS Region** select the region **Ohio**
    ![replicating](/images/secretmanager-replicating.png)

5.  Click **Complete adding region (s)** {{% notice tip%}}
    You can add more than one region for replication by clicking **Add more regions**
    {{% /notice%}}

Ready! You just replicated your Secret to another AWS region.

#### Viewing the result

1.  Select the region to which the Secret was replicated **Ohio**

2.  Find the Replicated Secret at **AWS Secrets Manager > Secrets**
    ![Secret Manager Replication](/images/secretmanager-replication-result.png)

{{% notice note%}}
The Replicated Secret is associated with Primary Secret, to make it independent just click **Promote to standalone secret**
{{% /notice%}}

#### Deleting resources

1.  Access the AWS Secrets Manager Service in the Primary Region **Virginia**: [https://console.aws.amazon.com/secretsmanager/](https://console.aws.amazon.com/secretsmanager/home?region=us-east-1)
2.  Click the created key.
3.  In the section **Replicate Secret**, select **Actions > Delete Replica** ![Delete Replica](/images/secretmanager-deletereplica.png)
4.  On the screen **Delete Replica** confirm by typing the region in which the replica should be deleted **us-east-2** and click **Delete Replica**.
5.  Go to the top of the page and click **Action > Delete Secret**.
6.  On the screen **Disable secret and schedule deletion**, confirm by clicking **Schedule deletion**
