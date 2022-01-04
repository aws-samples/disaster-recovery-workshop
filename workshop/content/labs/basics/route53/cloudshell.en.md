---
title: "Amazon Route 53 - Failover Policy in Private Hosted Zone"
menutitle: "Route 53 - Failover Policy in Private Hosted Zone"
weight: 1
pre: "<b></b>"
hidden: true
---

#### Go to AWS CloudShell

1.  Go to AWS CloudShell, in the top bar of the AWS Console, click the button on the right side of the search bar.
    ![CloudShell](/images/console-cloudshell2.png)

#### Create resources across multiple regions

1. Create a EC2 **Key Pair** for access the EC2 instance in each region.

   ```bash
   aws ec2 create-key-pair --key-name us-east-1-keypair --query 'KeyMaterial' --output text > us-east-1-keypair.pem --region us-east-1
   aws ec2 create-key-pair --key-name us-west-1-keypair --query 'KeyMaterial' --output text > us-west-1-keypair.pem --region us-west-1
   ```     

2. Create two VPC environments in two distinct regions with different CIDR (e.g. 10.0.0.0/16 and 10.1.0.0/16) with interconnectivity using the CloudFormation template below:


    ```bash
    # Create VPC and Subnets in N. Virginia
    aws cloudformation create-stack \
      --template-url https://dr-on-aws-workshop.s3.us-east-2.amazonaws.com/route53-vpc-cfn-template.yaml \
      --stack-name route53lab --parameters \
      ParameterKey=EnvironmentName,ParameterValue=DR-Workshop \
      ParameterKey=VpcCIDR,ParameterValue=10.0.0.0/16 \
      ParameterKey=PublicSubnet1CIDR,ParameterValue=10.0.1.0/24 \
      ParameterKey=KeyName,ParameterValue=us-east-1-keypair \
      --region us-east-1

    # Create VPC and Subnets in N. California
    aws cloudformation create-stack \
      --template-url https://dr-on-aws-workshop.s3.us-east-2.amazonaws.com/route53-vpc-cfn-template.yaml \
      --stack-name route53lab --parameters \
      ParameterKey=EnvironmentName,ParameterValue=DR-Workshop \
      ParameterKey=VpcCIDR,ParameterValue=10.1.0.0/16 \
      ParameterKey=PublicSubnet1CIDR,ParameterValue=10.1.1.0/24 \
      ParameterKey=KeyName,ParameterValue=us-west-1-keypair \
      --region us-west-1

    # Wait for CloudFormation stacks be completed
    aws cloudformation wait stack-create-complete \
      --stack-name route53lab --region us-east-1

    aws cloudformation describe-stacks --stack-name route53lab \
      --region us-east-1 | jq -r ".Stacks[].StackStatus"

    aws cloudformation wait stack-create-complete --stack-name route53lab \
      --region us-west-1

    aws cloudformation describe-stacks --stack-name route53lab \
      --region us-west-1 | jq -r ".Stacks[].StackStatus"
    ```

   {{% notice note %}}
   *Please wait about 5 minutes to resources be created on both regions.*
   {{% /notice %}}

#### Create a VPC Peering to connect both regions

1. Get the VPC ID of the VPC's created on previous step.

    ```bash
    export VPC_ID_PRIMARY=$(aws cloudformation describe-stacks --stack-name route53lab --region us-east-1 | jq -r .Stacks[0].Outputs[0].OutputValue)

    export VPC_ID_SECONDARY=$(aws cloudformation describe-stacks --stack-name route53lab --region us-west-1 | jq -r .Stacks[0].Outputs[0].OutputValue)

    echo PRIMARY VPC = $VPC_ID_PRIMARY
    echo SECONDARY VPC = $VPC_ID_SECONDARY
    
    ```

2. Create a VPC peering between the two VPCs from different AWS Regions.

    ```bash
    # Request VPC Peering
    export PEERING_ID=$(aws ec2 create-vpc-peering-connection \
      --vpc-id $VPC_ID_PRIMARY --region us-east-1 \
      --peer-vpc-id $VPC_ID_SECONDARY  --peer-region us-west-1 | \
      jq -r ".VpcPeeringConnection.VpcPeeringConnectionId")

    # Wait a few seconds
    sleep 10

    # Accept VPC Peering
    aws ec2 accept-vpc-peering-connection \
      --vpc-peering-connection-id $PEERING_ID \
      --region us-west-1
    ```

#### Create private DNS entries

1. Create a Route53 - Private Hosted Zone for the anycompany.internal DNS entries by associating the PRIMARY VPC in **us-east-1**.

    ```bash
    export HOSTED_ZONE_ID=$(aws route53 create-hosted-zone --name anycompany.internal \
      --caller-reference $(date "+%Y%m%d%H%M%S") \
      --hosted-zone-config PrivateZone=true \
      --vpc VPCRegion=us-east-1,VPCId=$VPC_ID_PRIMARY |\
      jq -r ".HostedZone.Id")
    ```

2. Associate **N. California** VPC with Private Hosted Zone to share DNS records

    ```bash
    aws route53 associate-vpc-with-hosted-zone \
      --hosted-zone-id $HOSTED_ZONE_ID \
      --vpc VPCRegion=us-west-1,VPCId=$VPC_ID_SECONDARY
    ```

#### Create Route53 Failover Policy

1. Get the private IP of both Web Servers.

    ```bash
    export WEBSERVER_PRIMARY_IP=$(aws ec2 describe-instances \
      --region us-east-1 \
      --filters \
      "Name=instance-state-name,Values=running" \
      "Name=tag-value,Values=WebServerInstance" \
      --query 'Reservations[*].Instances[*].[PublicIpAddress]' \
      --output text)

    export WEBSERVER_SECONDARY_IP=$(aws ec2 describe-instances \
      --region us-west-1 \
      --filters \
      "Name=instance-state-name,Values=running" \
      "Name=tag-value,Values=WebServerInstance" \
      --query 'Reservations[*].Instances[*].[PublicIpAddress]' \
      --output text)      

    echo PRIMARY WEB SERVER = $WEBSERVER_PRIMARY_IP
    echo SECONDARY WEB SERVER = $WEBSERVER_SECONDARY_IP  
    ```

2. Let's create the health check for our primary endpoint that will be in **us-east-1**
    ```bash
    # Health check policy
    cat > health-check-config.json << EOF
    {
      "Type": "HTTP",
      "Port": 80,
      "ResourcePath": "/index.html",
      "IPAddress": "$WEBSERVER_PRIMARY_IP",
      "RequestInterval": 30,
      "FailureThreshold": 3
    } 
    EOF
    ```

    ```bash
    # Create Healthcheck for Route53
    export HEALTH_ID=$(aws route53 create-health-check \
      --caller-reference $(date "+%Y%m%d%H%M%S") \
      --health-check-config file://health-check-config.json |\
      jq -r ".HealthCheck.Id")
    ```
    
    *The healthcheck will be active in 30 seconds.*

3. Creating our primary endpoint using Failover routing policy.

    ```bash
    # Failover policy
    cat > failover-policy.json << EOF
    {
        "AWSPolicyFormatVersion":"2015-10-01",
        "RecordType":"A",
        "StartRule":"site_switch",
        "Endpoints":{
            "WEBSERVER_PRIMARY":{
                "Type":"value",
                "Value":"$WEBSERVER_PRIMARY_IP"
            },
            "WEBSERVER_SECONDARY":{
                "Type":"value",
                "Value":"$WEBSERVER_SECONDARY_IP"
            }
        },
        "Rules":{
            "site_switch":{
                "RuleType":"failover",
                "Primary":{
                    "EndpointReference":"WEBSERVER_PRIMARY",
                    "HealthCheck": "$HEALTH_ID"
                },
                "Secondary":{
                    "EndpointReference":"WEBSERVER_SECONDARY"
                }
            }
        }
    }
    EOF
    ```

    ```bash
    # Create traffic policy
    export TRAFFIC_ID=$(aws route53 create-traffic-policy --name failover-policy \
      --document file://failover-policy.json | jq -r ".TrafficPolicy.Id")
    
    # Associate traffic policy to Private Hosted Zone
    aws route53 create-traffic-policy-instance \
      --hosted-zone-id $HOSTED_ZONE_ID --name service.anycompany.internal \
      --ttl 60 --traffic-policy-id $TRAFFIC_ID \
      --traffic-policy-version 1
    ```

    You just created a policy similar to the image below:
    ![Route Policy](/images/route53-policy.png)
    
#### Test the failover policy

1. Open a second terminal and access one EC2 instance by SSH
    
    Select *Actions* and select *Split into columns*
    ![Cloudshell](/images/cloudshell-split-columns.png)


    Using the second terminal, execute:
    ```bash
    # GET EC2 IP in N. Virginia
    export EC2_CLIENT_IP=$(aws ec2 describe-instances \
      --region us-east-1 \
      --filters \
      "Name=instance-state-name,Values=running" \
      "Name=tag-value,Values=EC2Client" \
      --query 'Reservations[*].Instances[*].[PublicIpAddress]' \
      --output text)
    
    # Access EC2 instance by SSH
    chmod 400 us-east-1-keypair.pem
    ssh -i us-east-1.pem ec2-user@$EC2_CLIENT_IP   
    ```

    *Answer 'yes' to add this EC2 instance to "Known hosts" file.

2. Try to access the website using "service.anycompany.internal"

    ```bash
    # Check the DNS answer
    dig +short service.anycompany.internal

    # Access the Web Server
    curl service.anycompany.internal
    ```

3. Using the first terminal, remove the inbound rule to primary Web Server

    ```bash
    # Get security group id of Web Server Primary
    export SG_ID_PRIMARY=$(aws ec2 describe-instances \
      --region us-east-1 \
      --filters \
      "Name=instance-state-name,Values=running" \
      "Name=tag-value,Values=WebServerInstance" \
      --query 'Reservations[*].Instances[*].NetworkInterfaces[*].Groups[*].GroupId' \
      --output text)

    # Revoke the inbound rule for **port 80**
    aws ec2 revoke-security-group-ingress \
      --group_id $SG_ID_PRIMARY \
      --ip-permissions FromPort=80,IpProtocol=tcp,ToPort=80,IpRanges=[{CidrIp=0.0.0.0/0}]
    ```

4. On second terminal, try to access the website again

   After 1 minute the DNS will answer the Secondary Web Server IP.

    ```bash
    # Check the DNS answer
    dig +short service.anycompany.internal

    # Access the Web Server
    curl service.anycompany.internal
    ```

#### Cleaning up

Delete all the resources    

```bash
# Delete VPC Peering Connection
aws ec2 delete-vpc-peering-connection --vpc-peering-connection-id $PEERING_ID

# Delete Cloudformation Stack
aws cloudformation delete-stack --stack-name route53lab --region us-east-1
aws cloudformation delete-stack --stack-name route53lab --region us-west-1

# Wait for conclusion
aws cloudformation wait stack-delete-complete --stack-name route53lab --region us-east-1
aws cloudformation wait stack-delete-complete --stack-name route53lab --region us-west-1

# Delete KeyPair
aws ec2 delete-key-pair --key-name us-east-1-keypair --region us-east-1
aws ec2 delete-key-pair --key-name us-west-1-keypair --region us-west-1
```