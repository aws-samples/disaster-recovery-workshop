---
hidden: true
type: lab
---

#### Create the First Bucket on Amazon S3

1.  Go to the Amazon S3 console: <https://s3.console.aws.amazon.com/s3>. <br>*Note: The Amazon S3 Service Console is Global, meaning buckets from all regions will be listed.*
2.  Choose **Create bucket (Create Bucket)**.
    *   Enter a name for your primary bucket (Example: **Bucket-Account-Number-Primary**
    *   Select the region **us-east-2 (Ohio)**
        ![create bucket](/images/s3-create-bucket.png)
    *   On the same page, enable the Bucket object versioning option.
        ![bucket versioning](/images/s3-bucket-versioning.png)
    *   Select the option **Create bucket (Create Bucket)**

#### Create a Second Bucket on Amazon S3

1.  Go to the Amazon S3 console: <https://s3.console.aws.amazon.com/s3>.
2.  Choose **Create bucket (Create Bucket)**.
    *   Enter a name for your primary bucket (Example: **Bucket-Account-Number-Secondary**
    *   Select the region **us-west-2 (Oregon**
    *   On the same page, enable the Bucket object versioning option.
    *   Select the option **Create bucket (Create Bucket)**

#### Primary Bucket Replication Rule for Secondary Bucket

1.  In the Amazon S3 console, select the first bucket created (**primary**).
2.  Go to the tab **Management**
3.  In the section **Replication rules (Replication rules)**, select **Create replication rule (Create Replication Rule)**.
    *   Enter a name for your rule (Example: **copy-to-secondary**).
        ![create replication](/images/s3-create-replication-rule.png)
    *   In the section **Source Bucket (source bucket)**, change the scope rule to the option **This rule applies to all objects in the bucket**
        ![replication scope](/images/s3-replication-scope.png)
    *   In the section **Destination** enter the name of your secondary bucket (Example:**Bucket-Account-Number-Secondary**).
        ![replication destination](/images/s3-replication-destination.png)
    *   In the section **IAM role** select the option **Create new role**.
        ![replication destination](/images/s3-replication-role.png)
        {{% notice note%}}
        Amazon S3 cannot replicate objects without your permission. You grant permissions with *roll* of the IAM specified in the replication configuration. Amazon S3 assumes *roll* from IAM to replicate objects on your behalf.
        {{% /notice%}}
    *   In the section **Additional replication options** Check all options as shown in the screenshot below:
        ![replication destination](/images/s3-replication-options.png)
    *   Select **Save (Save)**

#### Primary Bucket Replication Rule for Secondary Bucket

1.  In the Amazon S3 console, select the second bucket created (**secondary**).
2.  Go to the tab **Management**
3.  In the section **Replication rules (Replication rules)**, select **Create replication rule (Create Replication Rule)**.
    *   Enter a name for your rule (Example: **copy-to-primary**).
    *   In the section **Source Bucket (source bucket)**, change the scope rule to the option **This rule applies to all objects in the bucket**
    *   In the section **Destination** enter the name of your secondary bucket (Example:**Bucket-Account-Number-Primary**).
    *   In the section **IAM role** select the option **Create new role**.
    *   In the section **Additional replication options** Check all options as shown in the screenshot below:
    *   Select **Save (Save)**

#### Check replication rules

1.  Perform file upload to the primary bucket.
2.  Perform the upload of files to the secondary bucket.
3.  Verify that the files are copied to the secondary bucket.

{{% notice tip%}}
Cross-bucket replication is expected to occur in a maximum of 15 minutes for 99.99% of objects.
{{% /notice%}}

#### Cleaning up

1.  Delete the two created buckets.
