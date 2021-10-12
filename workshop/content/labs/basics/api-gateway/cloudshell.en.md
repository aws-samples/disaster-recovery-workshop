---
title: "Amazon API Gateway + Route53"
menutitle: "Amazon API Gateway + Route53"
weight: 1
pre: "<b></b>"
hidden: true
---


#### Go to AWS CloudShell

1. Go to AWS CloudShell, in the top bar of the AWS Console, click the button on the right side of the search bar.
    ![CloudShell](/images/console-cloudshell2.png)
2. Create a EC2 **Key Pair** for each region
   ```
   aws ec2 create-key-pair --key-name us-east-1-keypair --query 'KeyMaterial' --output text > us-east-1.pem --region us-east-1
   aws ec2 create-key-pair --key-name us-west-1-keypair --query 'KeyMaterial' --output text > us-west-1.pem --region us-west-1
   ```     

#### Build Resources Using AWS CloudFormation

1.  Create VPC. **Note**: Before you run AWS CloudFormation, for each region you use.

    ```bash
    # Create VPC and Subnets in N. Virginia (10.0.0.0/16)
    aws cloudformation create-stack \
      --template-url https://dr-on-aws-workshop.s3.us-east-2.amazonaws.com/apigw-api-cfn-template.yaml \
      --stack-name apigwlab --parameters \
      ParameterKey=EnvironmentName,ParameterValue=DR-Workshop \
      ParameterKey=VpcCIDR,ParameterValue=10.0.0.0/16 \
      ParameterKey=PublicSubnetCIDR,ParameterValue=10.0.1.0/24 \
      ParameterKey=PrivateSubnetCIDR,ParameterValue=10.0.2.0/24 \
      ParameterKey=KeyName,ParameterValue=us-east-1-keypair \
      --region us-east-1

    # Create VPC and Subnets in N. California (10.1.0.0/16)
    aws cloudformation create-stack \
      --template-url https://dr-on-aws-workshop.s3.us-east-2.amazonaws.com/apigw-api-cfn-template.yaml \
      --stack-name apigwlab --parameters \
      ParameterKey=EnvironmentName,ParameterValue=DR-Workshop \
      ParameterKey=VpcCIDR,ParameterValue=10.1.0.0/16 \
      ParameterKey=PublicSubnetCIDR,ParameterValue=10.1.1.0/24 \
      ParameterKey=PrivateSubnetCIDR,ParameterValue=10.1.2.0/24 \
      ParameterKey=KeyName,ParameterValue=us-west-1-keypair \
      --region us-west-1

    # Wait for CloudFormation stacks be completed
    aws cloudformation wait stack-create-complete \
      --stack-name apigwlab --region us-east-1

    aws cloudformation describe-stacks --stack-name apigwlab \
      --region us-east-1 | jq -r ".Stacks[].StackStatus"

    aws cloudformation wait stack-create-complete --stack-name apigwlab \
      --region us-west-1

    aws cloudformation describe-stacks --stack-name apigwlab \
      --region us-west-1 | jq -r ".Stacks[].StackStatus"
    ```

    {{% notice note %}}
   *Please wait about 5 minutes to resources be created on both regions.*
   {{% /notice %}}


#### Create connectivity to resources

1. Get the VPCs, subnets, and security groups created on previous step.

    ```
    # VPC ID - Primary Region
    export VPC_ID_PRIMARY=$(aws cloudformation describe-stacks --stack-name apigwlab --region us-east-1 | jq -r .Stacks[0].Outputs[1].OutputValue)

    # Private Subnet - Primary Region
    export PRIVATE_SUBNET_PRIMARY=$(aws cloudformation describe-stacks --stack-name apigwlab --region us-east-1 | jq -r .Stacks[0].Outputs[0].OutputValue)

    # Security Group - Primary Region
    export PRIVATE_SG_PRIMARY=$(aws cloudformation describe-stacks --stack-name apigwlab --region us-east-1 | jq -r .Stacks[0].Outputs[2].OutputValue)

    # VPC ID - Secondary Region
    export VPC_ID_SECONDARY=$(aws cloudformation describe-stacks --stack-name apigwlab --region us-west-1 | jq -r .Stacks[0].Outputs[1].OutputValue)

    # Private Subnet - Primary Region
    export PRIVATE_SUBNET_SECONDARY=$(aws cloudformation describe-stacks --stack-name apigwlab --region us-west-1 | jq -r .Stacks[0].Outputs[0].OutputValue)

    # Security Group - Secondary Region
    export PRIVATE_SG_SECONDARY=$(aws cloudformation describe-stacks --stack-name apigwlab --region us-west-1 | jq -r .Stacks[0].Outputs[2].OutputValue)l
    ```

2. Create VPC endpoints on VPCs created in the previous step.

    ```
    aws ec2 create-vpc-endpoint \
    --vpc-id $VPC_ID_PRIMARY \
    --vpc-endpoint-type Interface \
    --service-name com.amazonaws.us-east-1.execute-api \
    --subnet-id $PRIVATE_SUBNET_PRIMARY \
    --security-group-id $PRIVATE_SG_PRIMARY \
    --region us-east-1

    aws ec2 create-vpc-endpoint \
    --vpc-id $VPC_ID_SECONDARY \
    --vpc-endpoint-type Interface \
    --service-name com.amazonaws.us-west-1.execute-api \
    --subnet-id $PRIVATE_SUBNET_SECONDARY \
    --security-group-id $PRIVATE_SG_SECONDARY \
    --region us-west-1

    ```

2.  Create a sample private API in each region using the corresponding VPC Endpoint.

    *   Access the **Amazon API Gateway** console
    *   Select **Create API**
    *   In the list of types, select **REST API Private** and click **Import**.
    *   Click **OK**.
    *   Select the option **Example API** and click **Import.**
    *   On the API edit screen, click **Settings** below **Documentation**.
    *   Click on the text box for **VPC Endpoints** and select the previously created endpoint, click **Save Changes**.
    *   Click **Resource Policy** and click the Example button to **Source VPC Allowlist**, adjust the policy by including the VPC Endpoint Id as below, and click **Save**:

    <!---->

        {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Deny",
                    "Principal": "*",
                    "Action": "execute-api:Invoke",
                    "Resource": "execute-api:/*",
                    "Condition": {
                        "StringNotEquals": {
                            "aws:sourceVpce": "vpce-0f8bb1576e7b08d8a"
                        }
                    }
                },
                {
                    "Effect": "Allow",
                    "Principal": "*",
                    "Action": "execute-api:Invoke",
                    "Resource": "execute-api:/*"
                }
            ]
        }

    *   Deploy the API by creating a new stage called **prod**.

3.  Create a certificate in ACM in each region

    ```
    aws acm-pca create-certificate-authority \
    --certificate-authority-configuration file://ca_config.txt \
    --certificate-authority-type "ROOT" \
    --idempotency-token 98256344 \
    --region us-east-1

    aws acm-pca create-certificate-authority \
    --certificate-authority-configuration file://ca_config.txt \
    --certificate-authority-type "ROOT" \
    --idempotency-token 98256344 \
    --region us-west-1

    ```

    3a. Install a certificate for the CA created in the previous step.

    *   Sign in to the AWS console, access **Certificate Manager**.
    *   In **Private CAs**, select the previously created CA and select the menu **Actions -> Install CA certificate**.
    *   Click **Next**.
    *   Click **Confirm and Install**.

    3b. Request a private certificate for the private API domain.

    *   Sign in to the AWS console, access **Certificate Manager**
    *   In **Certificate Manager**, click **Getting Started**, select **Request a private certificate** and click **Request a certificate**.
    *   Select the previously created CA in the combobox (example.com), click **Acknowledge**, and then on **Next**.
    *   Fill with \***.example.com** upon **Domain name** and click **Next**.
    *   Click **Review and request**.
    *   Click **Confirm and request**.
    *   Copy the ARN into the details of the certificate created for use in the next step.

4.  Create a custom domain in API Gateway

*   Access the service **Amazon API Gateway** via the AWS console
*   Click **Custom domain names** on the left menu
*   In **Domain name**, includes the name of the domain to be used by the api, in this case **api.example.com**.
*   In **ACM certificate**, select the previously created certificate.
*   Click **Create domain name**.

    4th. Add a custom domain mapping to the available apis.

    *   Selecting the newly created custom domain, select the tab **API mappings** and click **Set up API mappings**.
    *   Click the button **Add new mapping**, and select:
        *   The name of the example API (**PetStore**)
        *   The stage where the API was published (**prod**)
    *   Click **Save**.

5.  Create NLB pointing to the VPC endpoints in each region. Repeat the commands below by swapping us-east-1 for us-west-1 and its features.

    ```
    aws elbv2 create-load-balancer --name api-nlb --type network --scheme internal --subnet-mappings SubnetId=<Id da Subnet privada> --region us-east-1

    aws elbv2 create-target-group \
    --name tg-api-private \
    --protocol TLS \
    --port 443 \
    --target-type ip \
    --vpc-id <Id da VPC> \
    --region us-east-1

    aws elbv2 register-targets \
    --target-group-arn <ARN do target group criado no passo anterior> \
    --targets Id=<IP do VPC Endpoint> \
    --region us-east-1

    aws elbv2 create-listener \
    --load-balancer-arn <ARN do load balancer> \
    --protocol TLS --port 443 --certificates CertificateArn=<ARN do certificado *.example.com> \
    --ssl-policy ELBSecurityPolicy-2016-08 \
    --default-actions Type=forward,TargetGroupArn=<ARN do target group> \
    --region us-east-1

    ```

6.  Create a VPC peering between the two VPCs from Different Regions

         aws ec2 create-vpc-peering-connection --vpc-id <vpcId da primeira região> --peer-vpc-id <vpcId da segunda região> --peer-region us-west-1 --region us-east-1

         aws ec2 accept-vpc-peering-connection --vpc-peering-connection-id <id do peering do passo anterior> --region us-west-1

    7.b. Create entries in the routing tables of both VPCs's public and private subnets pointing to VPC peering.

    | Subnet |Region: us-east-1 | Region: us-west-1 |
    |—|—|—|
    | Public/Private | Destination: 172.16.0.0/16, Target: PCX-xxxxx | Destination: 10.0.0.0/16, Target: PCX-YYYYY|

7.  Create a Private Hosted Zone for the internal domain example.com associating the VPC on us-east-1

    ```
    aws route53 create-hosted-zone --name example.com --caller-reference 2021-03-15-22:28 --hosted-zone-config PrivateZone=true --vpc VPCRegion=us-east-1,VPCId=<ID da VPC da região de Sao Paulo>

    ```

8.  Associate the us-west-1 VPC with Private Hosted Zone to share DNS records

    ```
    aws route53 associate-vpc-with-hosted-zone --hosted-zone-id <Id do Hosted Zone criado no passo anterior> --vpc VPCRegion=us-west-1,VPCId=<Id da VPC de N. California>

    ```

#### Set up Health Check

1.  Create a CloudWatch alarm-based health check for Route53 to identify if the endpoint is healthy. Run the following CloudFormation template with the name of hc-reliability stack and following values:

    *   Open the service **CloudWatch** on the AWS console.
    *   Click on the menu **Synthetics** -> **Canaries**
    *   Click **Create canary**.
    *   Select Blueprint **Heartbeat monitoring**.
    *   Enter a name for the canary. E.g.: canario-api-sp
    *   In **Application endpoint URL**, enter the default API endpoint into the region.
    *   Uncheck the option **Take screenshots**.
    *   In **Schedule**, select **Run continuously**.
        *   Set the run every 1 minute and check the box **Start immediately after creation**.
    *   Expand the CloudWatch Alarms section and click the Add new alarm button.
    *   Enter the values:
        *   Metric name: **Failed**
        *   Alarm condition: **Greater/Equal**
        *   Threshold: 1
        *   Average over period: **1 minute**
    *   Expand **VPC Settings** and select the VPC, Private Subnet, and Security Group from the us-east-1 region.
    *   Click **Create canary**.

2.  Create Route53 health check pointing to the alarm created by the canary from the previous step.

    *   Open the service **Route 53** on the AWS console.
    *   Expand side menu and select option **Health checks**.
    *   Click the button **Create health check**.
    *   Appoint the health check. E.g.: hc-api
    *   Select **State of CloudWatch alarm** upon **What to monitor**.
    *   Select the primary region. E.g. us-east-1
    *   Select the alarm created by the canary.
    *   Click **Next**.
    *   Click **Create health check**.
    *   Copy the health check ID to be used in Route 53 traffic policy.

3.  Set the routing policy by creating a politica.json file with the following content:

        {
            "AWSPolicyFormatVersion":"2015-10-01",
            "RecordType":"A",
            "StartRule":"site_switch",
            "Endpoints":{
                "my_ec2":{
                    "Type":"value",
                    "Value":"<IP privado da instância da região primária>"
                },
                "my_bkp_ec2":{
                    "Type":"value",
                    "Value":"<IP privado da instância da região secundária>"
                }
            },
            "Rules":{
                "site_switch":{
                    "RuleType":"failover",
                    "Primary":{
                        "EndpointReference":"my_ec2",
                        "HealthCheck": "<healthcheck obtido no comando anterior>"
                    },
                    "Secondary":{
                        "EndpointReference":"my_bkp_ec2"
                    }
                }
            }
        }

    ![Política de Roteamento](/images/apigw-policy.png)

        aws route53 create-traffic-policy --name minha_politica --document file://politica.json

    <!---->

        aws route53 create-traffic-policy-instance --hosted-zone-id <Id do Hosted Zone criado no passo 2> --name api.example.com --ttl 60 --traffic-policy-id <Id do Traffic Policy criado no passo anterior> --traffic-policy-version 1

#### Testing Cross-Region Routing

1.  Once DNS entries that use the failover policy between resources in different regions have been configured, simply trigger the alarm that indicates the failure in the health check of the primary environment that we can see that the DNS entry for the api.example.com endpoint will be switched to the other region.

#### Cleaning up

1.  Go to the CloudFormation Console in the Region **Sao Paulo**
2.  Select the **Stack** created.
3.  Click the button above **Delete**
4.  Confidence the button **Delete stack**
5.  Repeat steps 1 through 4 for the region **California** to erase the second **Stack**.
