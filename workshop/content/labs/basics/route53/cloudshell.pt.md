---
title: "Route 53 - Política de Failover em Private Hosted Zone"
menutitle: "Route 53 - Política de Failover em Private Hosted Zone"
weight: 1
pre: "<b></b>"
hidden: true
---

#### Acesse o AWS CloudShell

1. Acesse o AWS CloudShell, na barra superior do AWS Console, clique no botão ao lado direito da barra de busca.
   ![CloudShell](/images/console-cloudshell2.png)


#### Criar os recursos em múltiplas regiões  

1. Crie dois ambientes de VPC em duas regiões distintas com CIDR diferentes (p.ex. 10.0.0.0/16 e 172.16.0.0/16) com interconectividade usando o template CloudFormation abaixo:  

    | Região: N.Virginia (us-east-1)| Região: Sao Paulo (sa-east-1) |
    |-------------------|-------------------|
    |[![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fdr-on-aws-workshop.s3.us-east-2.amazonaws.com%2Froute53-vpc-cfn-template.yaml&stackName=route53lab) | [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=sa-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fdr-on-aws-workshop.s3.us-east-2.amazonaws.com%2Froute53-vpc-cfn-template.yaml&stackName=route53lab) |



    1b. Obter os id das VPCs criadas nas duas regiões:

        
        aws cloudformation describe-stacks --stack-name route53lab --region us-east-1 | jq -r .Stacks[0].Outputs[1].OutputValue

        aws cloudformation describe-stacks --stack-name route53lab --region sa-east-1 | jq -r .Stacks[0].Outputs[1].OutputValue



    Criar um VPC peering entre as duas VPCs de diferentes regiões

        aws ec2 create-vpc-peering-connection --vpc-id <vpcId da primeira região> --peer-vpc-id <vpcId da segunda região> --peer-region sa-east-1

        aws ec2 accept-vpc-peering-connection --vpc-peering-connection-id <id do peering do passo anterior> --region sa-east-1
    
    

2. Criar um Private Hosted Zone para o domínio interno customer.internal associando a VPC em sa-east-1

    ```
    aws route53 create-hosted-zone --name customer.internal --caller-reference 2021-03-15-22:28 --hosted-zone-config PrivateZone=true --vpc VPCRegion=sa-east-1,VPCId=<ID da VPC da região de São Paulo>

    ```

3. Associar a VPC de sa-east-1 com o Private Hosted Zone para compartilhar os registro de DNS

    ```
    aws route53 associate-vpc-with-hosted-zone --hosted-zone-id <Id do Hosted Zone criado no passo anterior> --vpc VPCRegion=sa-east-1,VPCId=<Id da VPC de N. Virginia>

    ```

4. Criar um health check baseado em alarme do CloudWatch para o Route 53 identificar se o endpoint está saudável. Executar o seguinte template de CloudFormation com o nome de stack hc-confiabilidade e seguintes valores:
 
 - Protocol: HTTP
 - IP Address: IP privado da instância primária do ambiente provisionado anteriormente
 - Port: 80
 - Path: Manter vazio
 - Lambda Subnet: Escolher a subnet privada da VPC do ambiente provisionado anteriormente
 - Lambda VPC: Escolhar a VPC do ambiente provisionado anteriormente

| Região: Sao Paulo (sa-east-1) |
    |-------------------|
    [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=sa-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fdr-on-aws-workshop.s3.us-east-2.amazonaws.com%2Froute53-private-hosted-zone.json&stackName=hc-confiabilidade) |


4.1. Obter o id do healthcheck e substituir na seguinte política do passo 5.
        
        aws cloudformation describe-stacks --stack-name hc-confiabilidade --region sa-east-1 | jq -r .Stacks[0].Outputs[0].OutputValue

        

1. Definir a política de roteamento criando um arquivo politica.json com o seguinte conteúdo:

    ```
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
    ```

    ![Política de Roteamento](/images/route53-policy.png)


    ```
    aws route53 create-traffic-policy --name minha_politica --document file://politica.json
    ```

    ```
    aws route53 create-traffic-policy-instance --hosted-zone-id <Id do Hosted Zone criado no passo 2> --name service.customer.internal --ttl 60 --traffic-policy-id <Id do Traffic Policy criado no passo anterior> --traffic-policy-version 1
    ```

2. Uma vez configuradas as entradas de DNS que utiliza a política de failover entre os recursos em diferentes regiões, basta disparar o alarme que indica a falha no health check do ambiente primário que podemos ver que a entrada DNS para o endpoint service.customer.internal será chaveado para a outra região. 

#### Apagando os recursos

1. Acesse a Console do CloudFormation na região **São Paulo**
2. Selecione a **Stack** criada. 
3. Clique no botão acima **Delete**
4. Confime no botão **Delete stack**
5. Repita os passos 1 ao 4 para a região **N. California** para apagar a segunda **Stack**.
