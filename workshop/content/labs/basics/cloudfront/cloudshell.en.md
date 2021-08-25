---
hidden: true
type: lab
---


#### 1. Go to AWS CloudShell

1. Go to AWS CloudShell, in the top bar of the AWS Console, click the button on the right side of the search bar.
2. 
   <img src="/images/console-cloudshell2.png?classes=shadow" />

   *Using AWS CloudShell we don't have to set up access though AWS CLI locally.*

#### 2. Create the configuration variables

1. Create the configuration variables to make it easier to use scripts
    ```bash
    ACCOUNT_ID=$(aws sts get-caller-identity | jq -r .Account) # AWS Account ID
    OHIO_LAB_BUCKET=${ACCOUNT_ID}-mini-lab-cf-ohio # Bucket Name - Primary Region
    OREGON_LAB_BUCKET=${ACCOUNT_ID}-mini-lab-cf-oregon # Bucket Name - Secondary Region
    ```
   {{% notice warning %}}
   *Stay tuned so you don't miss the [AWS CloudShell session](https://docs.aws.amazon.com/cloudshell/latest/userguide/limits.html#session-lifecycle-limitations) for inactivity, if the session is lost, the variables will be removed.*
   {{% /notice %}}

#### 3. Create the buckets on Amazon S3 to be the source

1. Create a bucket in the primary region, in this case **US East (Ohio)**.
    ```bash
    aws s3 mb s3://${OHIO_LAB_BUCKET} --region us-east-2
    ```

2. Create a bucket in the secondary region, in this case **US West (Oregon)**.
    ```bash
    aws s3 mb s3://${OREGON_LAB_BUCKET} --region us-west-2
    ```

3. Create the files to send to buckets
    ```bash
    echo "File in Ohio S3" > ohio.index.html
    echo "File in Oregon S3" > oregon.index.html
    ```
   *The content of the files is different to make it easy to known which AWS Region they come from.*

4. Copy the files to their buckets
    ```bash
    aws s3 cp ohio.index.html s3://${OHIO_LAB_BUCKET}/index.html
    aws s3 cp oregon.index.html s3://${OREGON_LAB_BUCKET}/index.html
    ```

#### 4. Create a **Origin Access Identity** on Amazon CloudFront

1. Create a  **Origin Access Identity** to have an identity and subsequently give access to the source buckets.
    ```bash
    OAI_ID=$(aws cloudfront create-cloud-front-origin-access-identity \
             --cloud-front-origin-access-identity-config \
             CallerReference="cloudfront-mini-lab-example",Comment="CloudFront Origin Group Example" \
             | jq -r '.CloudFrontOriginAccessIdentity.Id')
    ```
   {{% notice note%}}
   *It is made use of [Command Substitution](https://www.gnu.org/software/bash/manual/html_node/Command-Substitution.html) next to the command [jq](https://stedolan.github.io/jq/tutorial) to store the result of the command in a variable.*
   {{% /notice%}}

#### 5. Create Access Policies on Buckets to Allow Amazon CloudFront Access

1.  Create bucket access policies in the primary region, in this case **US East (Ohio)**.
    ```bash
    cat <<EOT > ohio-s3-policy.json
    {
       "Version": "2008-10-17",
       "Id": "PolicyForCloudFrontPrivateContent",
       "Statement": [
         {
           "Sid": "1",
           "Effect": "Allow",
           "Principal": {
             "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity ${OAI_ID}"
           },
           "Action": "s3:GetObject",
           "Resource": "arn:aws:s3:::${OHIO_LAB_BUCKET}/*"
         }
       ]
    }
    EOT
    ```

2.  Create bucket access policies in the secondary region, in this case **US West (Oregon)**.
    ```bash
    cat <<EOT > oregon-s3-policy.json
    {
       "Version": "2008-10-17",
       "Id": "PolicyForCloudFrontPrivateContent",
       "Statement": [
         {
           "Sid": "1",
           "Effect": "Allow",
           "Principal": {
             "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity ${OAI_ID}"
           },
           "Action": "s3:GetObject",
           "Resource": "arn:aws:s3:::${OREGON_LAB_BUCKET}/*"
         }
       ]
    }
    EOT
    ```

3.  Apply policies to buckets
    ```bash
    aws s3api put-bucket-policy \
        --bucket ${OHIO_LAB_BUCKET} --policy file://ohio-s3-policy.json
    aws s3api put-bucket-policy \
        --bucket ${OREGON_LAB_BUCKET} --policy file://oregon-s3-policy.json
    ```

#### 6. Create the Amazon CloudFront Distribution

1.  Create the distribution configuration file, this configuration will already create both sources (one for each bucket), and the source group with both sources, also disables the cache for easy testing.
    ```bash
    cat <<EOT > mini-lab-example.json
    {
      "CallerReference":"mini-lab-example",
      "Aliases":{
        "Quantity":0
      },
      "DefaultRootObject":"index.html",
      "Origins":{
        "Quantity":2,
        "Items":[
          {
            "Id":"mini-lab-origin-ohio",
            "DomainName":"${OHIO_LAB_BUCKET}.s3.amazonaws.com",
            "S3OriginConfig":{
              "OriginAccessIdentity":"origin-access-identity/cloudfront/${OAI_ID}"
            },
            "ConnectionAttempts":1,
            "ConnectionTimeout":2
          },
          {
            "Id":"mini-lab-origin-oregon",
            "DomainName":"${OREGON_LAB_BUCKET}.s3.amazonaws.com",
            "S3OriginConfig":{
              "OriginAccessIdentity":"origin-access-identity/cloudfront/${OAI_ID}"
            },
            "ConnectionAttempts":1,
            "ConnectionTimeout":2
          }
        ]
      },
      "OriginGroups":{
        "Quantity":1,
        "Items":[
          {
            "Id":"mini-lab-origin-group-example",
            "FailoverCriteria":{
              "StatusCodes":{
                "Quantity":6,
                "Items":[
                  404,
                  403,
                  500,
                  502,
                  503,
                  504
                ]
              }
            },
            "Members":{
              "Quantity":2,
              "Items":[
                {
                  "OriginId":"mini-lab-origin-ohio"
                },
                {
                  "OriginId":"mini-lab-origin-oregon"
                }
              ]
            }
          }
        ]
      },
      "DefaultCacheBehavior":{
        "TargetOriginId":"mini-lab-origin-group-example",
        "ForwardedValues":{
          "QueryString":false,
          "Cookies":{
            "Forward":"none"
          },
          "Headers":{
            "Quantity":0
          },
          "QueryStringCacheKeys":{
            "Quantity":0
          }
        },
        "TrustedSigners":{
          "Enabled":false,
          "Quantity":0
        },
        "TrustedKeyGroups":{
          "Enabled":false,
          "Quantity":0
        },
        "ViewerProtocolPolicy":"allow-all",
        "MinTTL":0,
        "MaxTTL":0,
        "DefaultTTL":0,
        "AllowedMethods":{
          "Quantity":2,
          "Items":[
            "HEAD",
            "GET"
          ],
          "CachedMethods":{
            "Quantity":2,
            "Items":[
              "HEAD",
              "GET"
            ]
          }
        }
      },
      "CacheBehaviors":{
        "Quantity":0
      },
      "Comment":"",
      "Enabled":true
    }
    EOT
    ```

2. Build the Distribution on Amazon CloudFront

    ```bash
    CLOUDFRONT_ID=$(aws cloudfront create-distribution \
                    --distribution-config file://mini-lab-example.json | jq -r '.Distribution.Id')
    ```

   {{% notice note%}}
   *After requesting the creation of the distribution, it is necessary to wait a few minutes for the distribution to be available.*
   {{% /notice%}}

3. (Optional) While you wait, you can check the distribution status with the command below.
    ```bash
    echo "Status: " $(aws cloudfront get-distribution \
                      --id ${CLOUDFRONT_ID} | jq -r '.Distribution.Status')
    ```

#### 7. Try the Origin Group

1. Take the URL of the distribution created in the previous step.
    ```bash
    CLOUDFRONT_URL=$(aws cloudfront get-distribution --id ${CLOUDFRONT_ID} | jq -r '.Distribution.DomainName')
    echo ${CLOUDFRONT_URL} 
    ```

2. Go to the URL to see the delivery of the primary region content.
    ```bash
    curl -s ${CLOUDFRONT_URL} # "File in Ohio S3"
    ```

3. Remove the index.html file from the primary region
    ```bash
    aws s3 rm s3://${OHIO_LAB_BUCKET}/index.html
    ```
   {{% notice note%}}
   *This step causes an 404 error. It will trigger the Failover mechanism.*
   {{% /notice %}}

4. Reaccess the URL of the distribution created on Amazon CloudFront to see delivery from the secondary region.
    ```bash
    curl -s ${CLOUDFRONT_URL} # "File in Oregon S3"
    ```

5. (Optional) copy the file back to the primary region but with another name
    ```bash
    aws s3 cp ohio.index.html s3://${OHIO_LAB_BUCKET}/ohio.html
    ```

6. (Optional) Access the file with the new name to see the return of the primary region
    ```bash
    curl -s ${CLOUDFRONT_URL}/ohio.html # "File in Ohio S3"
    ```
   {{% notice note%}}
   *This step shows that new requests will be directed to Primary Region first even after the previous request has failed.*
   {{% /notice %}}    

#### 8. Cleaning up

1. Disable distribution using the update distribution configuration feature.
   
    ```bash
    CLOUDFRONT_DIST=$(aws cloudfront get-distribution-config --id ${CLOUDFRONT_ID}) # Cloudfront Distribution info

    CLOUDFRONT_ETAG=$(echo $CLOUDFRONT_DIST | jq -r '.ETag') # Cloudfront ETag 

    echo $CLOUDFRONT_DIST | jq -r '.DistributionConfig' | jq -r .Enabled="false" > mini-lab-example-disabled.json # Change Enabled parameter to "false"

    CLOUDFRONT_ETAG=$(aws cloudfront update-distribution \
    --id ${CLOUDFRONT_ID} \
    --if-match ${CLOUDFRONT_ETAG} \
    --distribution-config file://mini-lab-example-disabled.json \
    | jq -r .ETag) 
    ```

2.  Wait a few minutes until the distribution is removed. You can check the deletion already finished using the command below:
    ```bash
    aws cloudfront get-distribution --id ${CLOUDFRONT_ID} | jq -r .Distribution.Status
    ```

3.  Remove the Amazon CloudFront distribution created for the lab.
    ```bash
    aws cloudfront delete-distribution --id ${CLOUDFRONT_ID} --if-match ${CLOUDFRONT_ETAG}
    ```

4.  Delete the Origin Access Identity used in the lab.
    ```bash
    OAI_ETAG=$(aws cloudfront get-cloud-front-origin-access-identity --id ${OAI_ID} | jq -r '.ETag')

    aws cloudfront delete-cloud-front-origin-access-identity \
    --id ${OAI_ID} --if-match ${OAI_ETAG}
    ```

5.  Delete buckets used in the lab in both regions.
    ```bash
    aws s3 rb s3://${OHIO_LAB_BUCKET} --force
    aws s3 rb s3://${OREGON_LAB_BUCKET} --force
    ```

6.  Delete the files created for the lab.
    ```bash
    rm ohio.index.html \
       oregon.index.html \
       ohio-s3-policy.json \
       oregon-s3-policy.json \
       mini-lab-example.json \
       mini-lab-example-disabled.json
    ```
