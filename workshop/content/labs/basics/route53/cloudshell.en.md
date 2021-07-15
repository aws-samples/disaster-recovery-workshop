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

1.  Create two VPC environments in two distinct regions with different CIDR (e.g. 10.0.0.0/16 and 172.16.0.0/16) with interconnectivity using the CloudFormation template below:

    | Region: N.Virginia (us-east-1) | Region: Sao Paulo (sa-east-1) |
    |-|-|
    |[![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fdr-on-aws-workshop.s3.us-east-2.amazonaws.com%2Froute53-vpc-cfn-template.yaml\&stackName=route53lab) | [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=sa-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fdr-on-aws-workshop.s3.us-east-2.amazonaws.com%2Froute53-vpc-cfn-template.yaml\&stackName=route53lab) |

    1b. Get the ID of the VPCs created in both regions:

         aws cloudformation describe-stacks --stack-name route53lab --region us-east-1 | jq -r .Stacks[0].Outputs[1].OutputValue

         aws cloudformation describe-stacks --stack-name route53lab --region sa-east-1 | jq -r .Stacks[0].Outputs[1].OutputValue

    Create a VPC peering between the two VPCs from Different Regions

         aws ec2 create-vpc-peering-connection --vpc-id <vpcId da primeira região> --peer-vpc-id <vpcId da segunda região> --peer-region sa-east-1

         aws ec2 accept-vpc-peering-connection --vpc-peering-connection-id <id do peering do passo anterior> --region sa-east-1

2.  Create a Private Hosted Zone for the customer.internal domain by associating the VPC in sa-east-1

    ```
    aws route53 create-hosted-zone --name customer.internal --caller-reference 2021-03-15-22:28 --hosted-zone-config PrivateZone=true --vpc VPCRegion=sa-east-1,VPCId=<ID da VPC da região de Sao Paulo>

    ```

3.  Associate sa-east-1 VPC with Private Hosted Zone to share DNS records

    ```
    aws route53 associate-vpc-with-hosted-zone --hosted-zone-id <Id do Hosted Zone criado no passo anterior> --vpc VPCRegion=sa-east-1,VPCId=<Id da VPC de N. Virginia>

    ```

4.  Create a CloudWatch alarm-based health check for Route53 to identify if the endpoint is healthy. Run the following CloudFormation template with the name of hc-reliability stack and following values:

*   Protocol: HTTP
*   IP Address: Private IP of the primary instance of the previously provisioned environment
*   Port: 80
*   Path: Keep Empty
*   Lambda Subnet: Choose the VPC Private Subnet from the Previously Provisioned Environment
*   Lambda VPC: Choose the VPC from the Previously Provisioned Environment

| Region: Sao Paulo (sa-east-1) |
|-|
[![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=sa-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fdr-on-aws-workshop.s3.us-east-2.amazonaws.com%2Froute53-private-hosted-zone.json\&stackName=hc-confiabilidade) |

4.1. Get the healthcheck id and replace it in the following policy from step 5.

```
    aws cloudformation describe-stacks --stack-name hc-confiabilidade --region sa-east-1 | jq -r .Stacks[0].Outputs[0].OutputValue

    
```

1.  Set the routing policy by creating a politica.json file with the following content:

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

    ![Política de Roteamento](/images/route53-policy.png)

        aws route53 create-traffic-policy --name minha_politica --document file://politica.json

    <!---->

        aws route53 create-traffic-policy-instance --hosted-zone-id <Id do Hosted Zone criado no passo 2> --name service.customer.internal --ttl 60 --traffic-policy-id <Id do Traffic Policy criado no passo anterior> --traffic-policy-version 1

2.  Once DNS entries that use the failover policy between resources in different regions are configured, simply trigger the alarm that indicates the health check failure of the primary environment that we can see that the DNS entry for the service.customer.internal endpoint will be switched to the other region.

#### Cleaning up

1.  Go to the CloudFormation Console in the Region **Sao Paulo**
2.  Select the **Stack** created.
3.  Click the button above **Delete**
4.  Confidence the button **Delete stack**
5.  Repeat steps 1 through 4 for the region **California** to erase the second **Stack**.
