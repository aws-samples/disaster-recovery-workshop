---
title: "Amazon API Gateway + Route53"
menutitle: "Amazon API Gateway + Route53"
weight: 1
pre: "<b></b>"
hidden: true
---


#### Go to AWS CloudShell

1. Sign in to the **AWS console**, change the region to N. Virginia (us-east-1) and access **AWS CloudShell**, in the top bar of the console.
    ![CloudShell](/images/console-cloudshell2.png)
2. Create a EC2 **Key Pair** for each region by using the commands bellow.
   ```
   aws ec2 create-key-pair --key-name us-east-1-keypair --query 'KeyMaterial' --output text > us-east-1.pem --region us-east-1
   aws ec2 create-key-pair --key-name us-west-1-keypair --query 'KeyMaterial' --output text > us-west-1.pem --region us-west-1
   ```     

#### Build Resources Using AWS CloudFormation

1.  Create the VPC on primary and secondary regions.

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

    # Private Subnet - Secondary Region
    export PRIVATE_SUBNET_SECONDARY=$(aws cloudformation describe-stacks --stack-name apigwlab --region us-west-1 | jq -r .Stacks[0].Outputs[0].OutputValue)

    # Security Group - Secondary Region
    export PRIVATE_SG_SECONDARY=$(aws cloudformation describe-stacks --stack-name apigwlab --region us-west-1 | jq -r .Stacks[0].Outputs[2].OutputValue)

    # Define the HostName as the Account Number.
    export HOST=$(aws sts get-caller-identity --query "Account" --output text)
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

    export PRIMARY_VPCENDPOINT_ID=$(aws ec2 describe-vpc-endpoints --region us-east-1| jq -r ".VpcEndpoints[].VpcEndpointId")

    aws ec2 create-vpc-endpoint \
    --vpc-id $VPC_ID_SECONDARY \
    --vpc-endpoint-type Interface \
    --service-name com.amazonaws.us-west-1.execute-api \
    --subnet-id $PRIVATE_SUBNET_SECONDARY \
    --security-group-id $PRIVATE_SG_SECONDARY \
    --region us-west-1

    export SECONDARY_VPCENDPOINT_ID=$(aws ec2 describe-vpc-endpoints --region us-west-1| jq -r ".VpcEndpoints[].VpcEndpointId")

    ```

3. Create a sample private API in each region using the corresponding VPC Endpoint.

    *   Access the Amazon **API Gateway** console and select the **Get Started** button.
    *   On the Left menu, select **APIs** and them select the **Build** button on REST API Private section.
    *   Select the option **Example API** and click **Import**.
    *   On the API edit screen (left side menu), click  **API: PetStore** >> **Settings**.
    *   Click on the text box for **VPC Endpoint IDs** and select the previously created endpoint (take a note of this VPC-Endpoint ID for the next steps), select **Save Changes**.
    *   On left menu, select **API: PetStore** >> **Resource Policy** and select the **Source VPC Allowlist** button, adjust the policy by including the VPC Endpoint Id as below, and select the **Save** button.

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
                            "aws:sourceVpce": "<<replace by VPC-Endpoint created>>"
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

    {{% notice info %}}
   *Change the **<<replace by VPC-Endpoint created>>** with the VPC Endpoint ID.*
   {{% /notice %}}

    * Deploy the API, select **Resources >> Actions >> Deploy API**. Creating a new stage called **prod** and select the **Deploy** button.
    
    {{% notice warning %}}
   *Repeat step 3 for N. California (us-west-1) region.*
   {{% /notice %}}

4. Create a certificate in AWS Certificate Manager (ACM) in each region.

    Create certificate authority configuration file using AWS CloudShell.
    ```
    cat <<EOT > ca_config.txt
    {
      "KeyAlgorithm":"RSA_2048",
      "SigningAlgorithm":"SHA256WITHRSA",
      "Subject":{
        "Country":"US",
        "Organization":"Example Corp",
        "OrganizationalUnit":"Sales",
        "State":"WA",
        "Locality":"Seattle",
        "CommonName":"www.example.com"
      }
    }
    EOT
    ```

    Create certificate authority on primary and secondary regions by executing the following command.
    ```
    export PRIMARY_PRIVATE_CA_ARN=$(aws acm-pca create-certificate-authority \
     --certificate-authority-configuration file://ca_config.txt \
     --certificate-authority-type "ROOT" \
     --idempotency-token 98256344 \
     --region us-east-1 | jq -r ".CertificateAuthorityArn")

    export SECONDARY_PRIVATE_CA_ARN=$(aws acm-pca create-certificate-authority \
     --certificate-authority-configuration file://ca_config.txt \
     --certificate-authority-type "ROOT" \
     --idempotency-token 98256344 \
     --region us-west-1 | jq -r ".CertificateAuthorityArn")
    ```

    Import a certificate authority for primary region.

    ```
    aws acm-pca get-certificate-authority-csr \
    --certificate-authority-arn $PRIMARY_PRIVATE_CA_ARN \
    --output text \
    --endpoint https://acm-pca.us-east-1.amazonaws.com \
    --region us-east-1 > ca.csr

    export PRIMARY_CERTIFICATE_ARN=$(aws acm-pca issue-certificate \
     --certificate-authority-arn $PRIMARY_PRIVATE_CA_ARN \
     --csr fileb://ca.csr \
     --signing-algorithm SHA256WITHRSA \
     --template-arn arn:aws:acm-pca:::template/RootCACertificate/V1 \
     --validity Value=10,Type=YEARS \
     --region us-east-1 | jq -r ".CertificateArn")

    # Wait a few seconds
    sleep 10

    aws acm-pca get-certificate \
	--certificate-authority-arn $PRIMARY_PRIVATE_CA_ARN \
	--certificate-arn $PRIMARY_CERTIFICATE_ARN \
	--region us-east-1 \
    --output text > cert.pem

    aws acm-pca import-certificate-authority-certificate \
     --certificate-authority-arn $PRIMARY_PRIVATE_CA_ARN \
     --certificate fileb://cert.pem \
     --region us-east-1

    aws acm-pca describe-certificate-authority \
	--certificate-authority-arn $PRIMARY_PRIVATE_CA_ARN \
	--region us-east-1 \
    --output json
    ```

    Import a certificate authority for secondary region.

    ```
    aws acm-pca get-certificate-authority-csr \
    --certificate-authority-arn $SECONDARY_PRIVATE_CA_ARN \
    --output text \
    --endpoint https://acm-pca.us-west-1.amazonaws.com \
    --region us-west-1 > ca.csr

    export SECONDARY_CERTIFICATE_ARN=$(aws acm-pca issue-certificate \
     --certificate-authority-arn $SECONDARY_PRIVATE_CA_ARN \
     --csr fileb://ca.csr \
     --signing-algorithm SHA256WITHRSA \
     --template-arn arn:aws:acm-pca:::template/RootCACertificate/V1 \
     --validity Value=10,Type=YEARS \
     --region us-west-1 | jq -r ".CertificateArn")
    
    # Wait a few seconds
    sleep 10

    aws acm-pca get-certificate \
	--certificate-authority-arn $SECONDARY_PRIVATE_CA_ARN \
	--certificate-arn $SECONDARY_CERTIFICATE_ARN \
	--region us-west-1 \
    --output text > cert.pem

    aws acm-pca import-certificate-authority-certificate \
     --certificate-authority-arn $SECONDARY_PRIVATE_CA_ARN \
     --certificate fileb://cert.pem \
     --region us-west-1

    aws acm-pca describe-certificate-authority \
	--certificate-authority-arn $SECONDARY_PRIVATE_CA_ARN \
	--region us-west-1 \
    --output json
    ```

    Request a Certificate for both regions.

    ```
    aws acm request-certificate \
    --domain-name "*.example.com" \
    --subject-alternative-names $HOST".example.com" \
    --certificate-authority-arn $PRIMARY_PRIVATE_CA_ARN \
    --region us-east-1 \
    --output json

    aws acm request-certificate \
    --domain-name "*.example.com" \
    --subject-alternative-names $HOST".example.com" \
    --certificate-authority-arn $SECONDARY_PRIVATE_CA_ARN \
    --region us-west-1 \
    --output json
    ```

5. Create a custom domain in API Gateway (primary and secondary regions)

    Get some environment info by creating some environment variables.

    ```
    export PRIMARY_CERT_ARN=$(aws acm list-certificates --region us-east-1 | jq -r ".CertificateSummaryList[].CertificateArn")
    export SECONDARY_CERT_ARN=$(aws acm list-certificates --region us-west-1 | jq -r ".CertificateSummaryList[].CertificateArn")
    export PRIMARY_API_ID=$(aws apigateway get-rest-apis --region us-east-1 | jq -r ".items[].id")
    export SECONDARY_API_ID=$(aws apigateway get-rest-apis --region us-west-1 | jq -r ".items[].id")
    ```
    
    Create a custom domain and API mapping in API Gateway.
    ```
    aws apigatewayv2 create-domain-name \
    --domain-name $HOST".example.com" \
    --region us-east-1 \
    --domain-name-configurations CertificateArn=$PRIMARY_CERT_ARN

    aws apigatewayv2 create-api-mapping \
    --domain-name $HOST".example.com" \
    --api-id $PRIMARY_API_ID \
    --stage 'prod' \
    --region us-east-1

    aws apigatewayv2 create-domain-name \
    --domain-name $HOST".example.com" \
    --region us-west-1 \
    --domain-name-configurations CertificateArn=$SECONDARY_CERT_ARN

    aws apigatewayv2 create-api-mapping \
    --domain-name $HOST".example.com" \
    --api-id $SECONDARY_API_ID \
    --stage 'prod' \
    --region us-west-1
    ```

7. Create NLB pointing to the VPC endpoints in each region. 

   Primary Load Balancer and Target Group.

    ```bash
    # Load Balancer - Primary
    export LB_ARN_PRIMARY=$(aws elbv2 create-load-balancer \
    --name api-nlb --type network --scheme internal \
    --subnet-mappings SubnetId=$PRIVATE_SUBNET_PRIMARY \
    --region us-east-1 | jq -r .LoadBalancers[0].LoadBalancerArn)

    export TG_ARN_PRIMARY=$(aws elbv2 create-target-group \
    --name tg-api-private \
    --protocol TLS \
    --port 443 \
    --target-type ip \
    --vpc-id $VPC_ID_PRIMARY \
    --region us-east-1 | jq -r .TargetGroups[0].TargetGroupArn)

    export VPC_ENDPOINT_ENI_PRIMARY=$(aws ec2 describe-vpc-endpoints --region us-east-1 | jq -r .VpcEndpoints[0].NetworkInterfaceIds[0])

    export VPC_ENDPOINT_IP_PRIMARY=$(aws ec2 describe-network-interfaces --filters Name=network-interface-id,Values=$VPC_ENDPOINT_ENI_PRIMARY --region us-east-1 | jq -r .NetworkInterfaces[0].PrivateIpAddress)

    aws elbv2 register-targets \
    --target-group-arn $TG_ARN_PRIMARY \
    --targets Id=$VPC_ENDPOINT_IP_PRIMARY \
    --region us-east-1

    aws elbv2 create-listener \
    --load-balancer-arn $LB_ARN_PRIMARY \
    --protocol TLS --port 443 --certificates CertificateArn=$PRIMARY_CERT_ARN \
    --ssl-policy ELBSecurityPolicy-2016-08 \
    --default-actions Type=forward,TargetGroupArn=$TG_ARN_PRIMARY \
    --region us-east-1
    ```

    Secondary Load Balancer and Target Group.

    ```bash
    # Load Balancer - Secondary
    export LB_ARN_SECONDARY=$(aws elbv2 create-load-balancer \
    --name api-nlb --type network --scheme internal \
    --subnet-mappings SubnetId=$PRIVATE_SUBNET_SECONDARY \
    --region us-west-1 | jq -r .LoadBalancers[0].LoadBalancerArn)

    export TG_ARN_SECONDARY=$(aws elbv2 create-target-group \
    --name tg-api-private \
    --protocol TLS \
    --port 443 \
    --target-type ip \
    --vpc-id $VPC_ID_SECONDARY \
    --region us-west-1 | jq -r .TargetGroups[0].TargetGroupArn)

    export VPC_ENDPOINT_ENI_SECONDARY=$(aws ec2 describe-vpc-endpoints --region us-west-1 | jq -r .VpcEndpoints[0].NetworkInterfaceIds[0])

    export VPC_ENDPOINT_IP_SECONDARY=$(aws ec2 describe-network-interfaces --filters Name=network-interface-id,Values=$VPC_ENDPOINT_ENI_SECONDARY --region us-west-1 | jq -r .NetworkInterfaces[0].PrivateIpAddress)

    aws elbv2 register-targets \
    --target-group-arn $TG_ARN_SECONDARY \
    --targets Id=$VPC_ENDPOINT_IP_SECONDARY \
    --region us-west-1

    aws elbv2 create-listener \
    --load-balancer-arn $LB_ARN_SECONDARY \
    --protocol TLS --port 443 --certificates CertificateArn=$SECONDARY_CERT_ARN \
    --ssl-policy ELBSecurityPolicy-2016-08 \
    --default-actions Type=forward,TargetGroupArn=$TG_ARN_SECONDARY \
    --region us-west-1
    ```

8. Create a VPC peering between the two VPCs from Different Regions.

    ```bash
    # Request VPC Peering
    aws ec2 create-vpc-peering-connection \
      --vpc-id $VPC_ID_PRIMARY --region us-east-1 \
      --peer-vpc-id $VPC_ID_SECONDARY  --peer-region us-west-1 | \
      jq -r ".VpcPeeringConnection.VpcPeeringConnectionId"
    
    export PEERING_ID=$(aws ec2 describe-vpc-peering-connections | jq -r ".VpcPeeringConnections[].VpcPeeringConnectionId")

    # Wait a few seconds
    sleep 10

    # Accept VPC Peering
    aws ec2 accept-vpc-peering-connection \
      --vpc-peering-connection-id $PEERING_ID \
      --region us-west-1
    ```

9. Adjust the VPC Route Table, on both regions, to use VPC Peering.

    ```
    #Creating some environment Variables
    export RT_PRIMARY_PRIVATE=$(aws ec2 describe-route-tables --region us-east-1 --filter 'Name=tag:Name,Values=DR-Workshop Private Routes' | jq -r ".RouteTables[].Associations[].RouteTableId")
    
    export RT_PRIMARY_PUBLIC=$(aws ec2 describe-route-tables --region us-east-1 --filter 'Name=tag:Name,Values=DR-Workshop Public Routes' | jq -r ".RouteTables[].Associations[].RouteTableId")
    
    export RT_SECONDARY_PRIVATE=$(aws ec2 describe-route-tables --region us-west-1 --filter 'Name=tag:Name,Values=DR-Workshop Private Routes' | jq -r ".RouteTables[].Associations[].RouteTableId")
    
    export RT_SECONDARY_PUBLIC=$(aws ec2 describe-route-tables --region us-west-1 --filter 'Name=tag:Name,Values=DR-Workshop Public Routes' | jq -r ".RouteTables[].Associations[].RouteTableId")

    #Adding routes to the current Route Tables (Primary and Secondary regions)
    aws ec2 create-route --route-table-id $RT_PRIMARY_PRIVATE \
    --destination-cidr-block "10.1.0.0/16" \
    --vpc-peering-connection-id $PEERING_ID \
    --region us-east-1
    
    aws ec2 create-route --route-table-id $RT_PRIMARY_PUBLIC \
    --destination-cidr-block "10.1.0.0/16" \
    --vpc-peering-connection-id $PEERING_ID \
    --region us-east-1
    
    aws ec2 create-route --route-table-id $RT_SECONDARY_PRIVATE \
    --destination-cidr-block "10.0.0.0/16" \
    --vpc-peering-connection-id $PEERING_ID \
    --region us-west-1
    
    aws ec2 create-route --route-table-id $RT_SECONDARY_PUBLIC \
    --destination-cidr-block "10.0.0.0/16" \
    --vpc-peering-connection-id $PEERING_ID \
    --region us-west-1
    ```

10. Create a Private Hosted Zone for the internal domain example.com associating the VPC on us-east-1.

    ```bash
    aws route53 create-hosted-zone --name example.com \
      --caller-reference $(date "+%Y%m%d%H%M%S") \
      --hosted-zone-config PrivateZone=true \
      --vpc VPCRegion=us-east-1,VPCId=$VPC_ID_PRIMARY |\
      jq -r ".HostedZone.Id"
    
    export HOSTED_ZONE_ID=$(aws route53 list-hosted-zones | jq -r ".HostedZones[].Id")
    ```

11. Associate the us-west-1 VPC with Private Hosted Zone to share DNS records.

    ```bash
    aws route53 associate-vpc-with-hosted-zone \
      --hosted-zone-id $HOSTED_ZONE_ID \
      --vpc VPCRegion=us-west-1,VPCId=$VPC_ID_SECONDARY
    ```

#### Set up Health Check

1. Create a CloudWatch alarm-based health check for Route53 to identify if the endpoint is healthy.

    *   Open the service **CloudWatch** on the AWS console and change to the primary region (us-east-1).
    *   On the left menu, select **Application monitoring** >> **Synthetics Canaries**
    * Select **Create canary** button and then select the blueprint **Heartbeat monitoring**.
    * Enter a name for the canary on the **Name** field. E.g.: canario-api
    * In **Application endpoint URL** field, enter the default API endpoint into the region. Please select the **PRIMARY** (US-EAST-1) region's API Endpoint (API Gateway >> "PetStore" >> Dashboard >> Invoke URL)
    * Uncheck the option **Take screenshots**.
    * In *Schedule*, select **Run continuously**.
        * Set the run every 1 minute and check the box **Start immediately after creation**.
    * Expand the **CloudWatch Alarms** section and click the **Add new alarm** button.
    * Enter the values:
        * Metric name: **Failed**
        * Alarm condition: **Greater/Equal**
        * Threshold: 1
        * Average over period: **1 minute**
    * Expand **VPC Settings** and select the VPC (with 10.0.0.0/16 CIDR), Private Subnet, and Security Group (Default) from the us-east-1 region.
    * Click **Create canary**.

    {{% notice warning %}}
   *Wait for the completion of this task before proceed. This activity may take 4-5 minutes to complete.*
   {{% /notice %}}

2. Create Route53 health check pointing to the alarm created by the canary from the previous step.

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

3. To create a routing policy, first, we will create a *policy.json* file using **CloudShell**:

    ```
    # Get the Route53 Health Check Id
    export HEALTH_CHECK_ID=$(aws route53 list-health-checks | jq -r ".HealthChecks[].Id")
    
    # Create the Policy.json file
    cat << EOF > policy.json
        {
            "AWSPolicyFormatVersion":"2015-10-01",
            "RecordType":"A",
            "StartRule":"site_switch",
            "Endpoints":{
                "my_ec2":{
                    "Type":"value",
                    "Value":"$VPC_ENDPOINT_IP_PRIMARY"
                },
                "my_bkp_ec2":{
                    "Type":"value",
                    "Value":"$VPC_ENDPOINT_IP_SECONDARY"
                }
            },
            "Rules":{
                "site_switch":{
                    "RuleType":"failover",
                    "Primary":{
                        "EndpointReference":"my_ec2",
                        "HealthCheck": "$HEALTH_CHECK_ID"
                    },
                    "Secondary":{
                        "EndpointReference":"my_bkp_ec2"
                    }
                }
            }
        }
    EOF
    
    # Create Route53 Traffic Policy based on policy.json file
    aws route53 create-traffic-policy --name my-policy --document file://policy.json

    export TRAFFIC_POLICY_ID=$(aws route53 list-traffic-policies | jq -r ".TrafficPolicySummaries[].Id")

    # Create Route53 Traffic Policy Instance
    aws route53 create-traffic-policy-instance --hosted-zone-id $HOSTED_ZONE_ID --name $HOST.example.com --ttl 60 --traffic-policy-id $TRAFFIC_POLICY_ID --traffic-policy-version 1
    ```



#### Testing Cross-Region Routing

1. Once DNS entries that use the failover policy between resources in different regions have been configured, we will trigger the alarm that indicates the failure in the health check of the primary environment that we can see that the DNS entry for the petstore.example.com endpoint will be switched to the other region.

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
    chmod 400 us-east-1.pem
    ssh -i us-east-1.pem ec2-user@$EC2_CLIENT_IP   
    ```

    {{% notice info %}}
   *Answer 'yes' to add this EC2 instance to "Known hosts" file.
   {{% /notice %}}

2. Try to access the website using "<AccountNumber>.example.com"

    ```bash
    # Check the DNS answer
    dig +short <AccountNumber>.example.com

    # Access the Web Server
    curl -k https://<AccountNumber>.example.com
    ```
    {{% notice warning %}}
   *Change the **<<AccountNumber>>** with the AWS Account Number. E.g. 1234123443*
   {{% /notice %}}

3. Using the first terminal, remove the inbound rule to VPC Endpoint.

    ```bash
    # Get security group id of Web Server Primary
    export SG_ID_PRIMARY=$(aws ec2 describe-security-groups \
    --filters "Name=group-name,Values=apigwlab-VpceSecurityGroup-119XHWAQ256WC" |\
     jq -r ".SecurityGroups[].GroupId")

    # Revoke the inbound rule for **port 80**
    aws ec2 revoke-security-group-ingress \
      --group-id $SG_ID_PRIMARY \
      --ip-permissions FromPort=443,IpProtocol=tcp,ToPort=443,IpRanges=[{CidrIp=0.0.0.0/0}]
    ```

4. Waite for a couple of minutes and, on the second terminal, try to access the website again

    ```bash
    # Check the DNS answer
    dig +short <AccountNumber>.example.com

    # Access the Web Server
    curl -k https://<AccountNumber>.example.com
    ```
    {{% notice warning %}}
   *Change the **<<AccountNumber>>** with the AWS Account Number. E.g. 1234123443*
   {{% /notice %}}

    {{% notice info %}}
   *Note that the IP returned by the dig command changed!*
   {{% /notice %}}

#### Cleaning up

1. Go to the CloudFormation Console in the Region **N. Virginia**
2. Select the **Stack** created.
3. Click the button above **Delete**
4. Confidence the button **Delete stack**
5. Repeat steps 1 through 4 for the region **N. California** to erase the second **Stack**.
