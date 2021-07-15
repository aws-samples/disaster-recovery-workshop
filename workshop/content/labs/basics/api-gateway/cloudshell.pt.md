---
title: "Amazon API Gateway + Route53"
menutitle: "Amazon API Gateway + Route53"
weight: 1
pre: "<b></b>"
hidden: true
---

#### Acesse o AWS CloudShell

1. Acesse o AWS CloudShell, na barra superior do AWS Console, clique no botão ao lado direito da barra de busca.
   ![CloudShell](/images/console-cloudshell2.png)


#### Crie os recursos usando o AWS CloudFormation

1. Crie uma **Key Pair** para cada região (caso não exista). Será utilizada para acessar as instâncias EC2.
2. Criar VPC. **Observação**: Antes de executar o AWS CloudFormation, para cada região utilizada.

    | Região: São Paulo (sa-east-1)| Região: N. California (us-west-1) |
    |-------------------|-------------------|
    |[![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=sa-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fdr-on-aws-workshop.s3.us-east-2.amazonaws.com%2Fapigw-api-cfn-template.yaml&stackName=api-vpc) |[![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-west-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fdr-on-aws-workshop.s3.us-east-2.amazonaws.com%2Fapigw-api-cfn-template.yaml&stackName=api-vpc) |

    2b. Obter os ids das VPCs, subnets privadas e security groups criados nas duas regiões:
        
        aws cloudformation describe-stacks --stack-name api-vpc --region sa-east-1 | jq -r .Stacks[0].Outputs[0].OutputValue
        aws cloudformation describe-stacks --stack-name api-vpc --region sa-east-1 | jq -r .Stacks[0].Outputs[1].OutputValue
        aws cloudformation describe-stacks --stack-name api-vpc --region sa-east-1 | jq -r .Stacks[0].Outputs[2].OutputValue

        aws cloudformation describe-stacks --stack-name api-vpc --region us-west-1 | jq -r .Stacks[0].Outputs[0].OutputValue
        aws cloudformation describe-stacks --stack-name api-vpc --region us-west-1 | jq -r .Stacks[0].Outputs[1].OutputValue
        aws cloudformation describe-stacks --stack-name api-vpc --region us-west-1 | jq -r .Stacks[0].Outputs[2].OutputValue
        
#### Crie a conectividade para os recursos 
1. Criar VPC endpoints nas VPCs criadas no passo anterior.

    ```
    aws ec2 create-vpc-endpoint \
    --vpc-id <Id da VPC na região 1> \
    --vpc-endpoint-type Interface \
    --service-name com.amazonaws.sa-east-1.execute-api \
    --subnet-id <Subnet privada da região 1> \
    --security-group-id <Security Group da região 1 para acesso HTTPS> \
    --region sa-east-1

    aws ec2 create-vpc-endpoint \
    --vpc-id <Id da VPC na região 1> \
    --vpc-endpoint-type Interface \
    --service-name com.amazonaws.us-west-1.execute-api \
    --subnet-id <Subnet privada da região 1> \
    --security-group-id <Security Group da região 1 para acesso HTTPS> \
    --region us-west-1

    ```

2. Criar uma API privada de exemplo em cada região usando o VPC Endpoint correspondente.


    - Acessar o menu do **Amazon API Gateway**
    - Na lista de tipos, selecionar **REST API Private** e clicar em **Import**.
    - Clicar em **OK**.
    - Selecionar a opção **Example API** e clicar em **Import.**
    - Na tela de edição da API, clicar em **Settings** abaixo de **Documentation**.
    - Clicar sobre a caixa de texto de **VPC Endpoints** e selecionar o endpoint criado anteriormente, clicar em **Save Changes**.
    - Clicar em **Resource Policy** e clicar no botão de Exemplo para **Source VPC Allowlist**, ajustar a política incluindo o Id do Endpoint da VPC conforme abaixo e clicar em **Save**:

    ```
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
    ```

    - Fazer o deploy da API criando um novo stage chamado **prod**.


3. Criar um certificado no ACM em cada região

    ```
    aws acm-pca create-certificate-authority \
	--certificate-authority-configuration file://ca_config.txt \
	--certificate-authority-type "ROOT" \
	--idempotency-token 98256344 \
    --region sa-east-1

    aws acm-pca create-certificate-authority \
	--certificate-authority-configuration file://ca_config.txt \
	--certificate-authority-type "ROOT" \
	--idempotency-token 98256344 \
    --region us-west-1

    ```

    3a. Instalar um certificado para o CA criado no passo anterior. 

    - Entrar no console AWS, acessar **Certificate Manager**. 
    - Em **Private CAs**, selecionar a CA criada anteriormente e selecionar o menu **Actions -> Install CA certificate**. 
    - Clicar em **Next**.
    - Clicar em **Confirm and Install**.   

    3b. Requisitar um certificado privado para o domínio da API privada.

    - Entrar no console AWS, acessar **Certificate Manager**
    - Em **Certificate Manager**, cliar em **Getting Started**, selecionar **Request a private certificate** e clicar em **Request a certificate**.
    - Selecionar a CA criada anteriormente no combobox (example.com), clicar em **Acknowledge** e depois em **Next**.
    - Preencher com ***.example.com** em **Domain name** e clicar em **Next**.
    - Clicar em **Review and request**.
    - Clicar em **Confirm and request**.
    - Copiar o ARN nos detalhes do certificado criado para usar no passo seguinte.

4. Criar um custom domain no API Gateway

- Acessar o serviço do **Amazon API Gateway** via console AWS
- Clicar em **Custom domain names** no menu esquerdo
- Em **Domain name**, inclui o nome do domínio a ser utilizado pela api, no caso **api.example.com**.
- Em **ACM certificate**, selecionar o certificado criado anteriormente.
- Clicar em **Create domain name**.

    4a. Adicionar um mapeamento do custom domain para as apis disponíveis.

    - Selecionando o custom domain recém-criado, selecionar a aba **API mappings** e clicar em **Configure API mappings**.
    - Clicar no botão **Add new mapping**, e selecionar:
        - O nome da API de exemplo (**PetStore**)
        - O stage onde a API foi publicada (**prod**)
    - Clicar em **Save**.

5. Criar NLB apontando para os VPC endpoints em cada região. Repetir os comandos abaixo trocando sa-east-1 por us-west-1 e respectivos recursos.

    ```
    aws elbv2 create-load-balancer --name api-nlb --type network --scheme internal --subnet-mappings SubnetId=<Id da Subnet privada> --region sa-east-1

    aws elbv2 create-target-group \
    --name tg-api-private \
    --protocol TLS \
    --port 443 \
    --target-type ip \
    --vpc-id <Id da VPC> \
    --region sa-east-1

    aws elbv2 register-targets \
    --target-group-arn <ARN do target group criado no passo anterior> \
    --targets Id=<IP do VPC Endpoint> \
    --region sa-east-1

    aws elbv2 create-listener \
    --load-balancer-arn <ARN do load balancer> \
    --protocol TLS --port 443 --certificates CertificateArn=<ARN do certificado *.example.com> \
    --ssl-policy ELBSecurityPolicy-2016-08 \
    --default-actions Type=forward,TargetGroupArn=<ARN do target group> \
    --region sa-east-1

    ```

6. Criar um VPC peering entre as duas VPCs de diferentes regiões

        aws ec2 create-vpc-peering-connection --vpc-id <vpcId da primeira região> --peer-vpc-id <vpcId da segunda região> --peer-region us-west-1 --region sa-east-1

        aws ec2 accept-vpc-peering-connection --vpc-peering-connection-id <id do peering do passo anterior> --region us-west-1
    
    7.b. Criar entradas nas tabelas de roteamento das subnets públicas e privadas das duas VPCs apontando para o VPC peering.

    | Subnet |Região: sa-east-1 | Região: us-west-1 |
    |--------|------------------|-------------------|
    | publica/privada | Destination: 172.16.0.0/16, Target: pcx-XXXXX | Destination: 10.0.0.0/16, Target: pcx-YYYYY|


7. Criar um Private Hosted Zone para o domínio interno example.com associando a VPC em sa-east-1

    ```
    aws route53 create-hosted-zone --name example.com --caller-reference 2021-03-15-22:28 --hosted-zone-config PrivateZone=true --vpc VPCRegion=sa-east-1,VPCId=<ID da VPC da região de São Paulo>

    ```

8. Associar a VPC de us-west-1 com o Private Hosted Zone para compartilhar os registro de DNS

    ```
    aws route53 associate-vpc-with-hosted-zone --hosted-zone-id <Id do Hosted Zone criado no passo anterior> --vpc VPCRegion=us-west-1,VPCId=<Id da VPC de N. California>

    ```

#### Configure o Health Check
1. Criar um health check baseado em alarme do CloudWatch para o Route53 identificar se o endpoint está saudável. Executar o seguinte template de CloudFormation com o nome de stack hc-confiabilidade e seguintes valores:

    - Abrir o serviço **CloudWatch** no console AWS.
    - Clicar no menu **Synthetics** -> **Canaries**
    - Clicar em **Create canary**.
    - Selecionar o Blueprint **Heartbeat monitoring**.
    - Inserir um nome para o canário. P.ex: canario-api-sp
    - Em **Application endpoint URL**, inserir o endpoint default da API na região.
    - Desmarcar a opção **Take screenshots**.
    - Em **Schedule**, selecionar **Run continuously**.
        - Definir a execução a cada 1 minuto e marcar a opção **Start immediately after creation**.
    - Expandir a seção CloudWatch alarms e clicar no botão Add new alarm.
    - Entrar com os valores: 
        - Metric name: **Failed**
        - Alarm condition: **Greater/Equal**
        - Threshold: 1
        - Average over period: **1 minute**
    - Expandir **VPC Settings** e selecionar a VPC, Subnet privada e Security Group da região sa-east-1.
    - Clicar em **Create canary**.

2. Criar health check do Route53 apontando para o alarme criado pelo canário do passo anterior.

    - Abrir o serviço **Route 53** no console AWS.
    - Expandir menu lateral e selecionar opção **Health checks**.
    - Clicar no botão **Create health check**.
    - Nomear o health check. P.ex.: hc-api
    - Selecionar **State of CloudWatch alarm** em **What to monitor**.
    - Selecionar a região primária. P.ex. sa-east-1
    - Selecionar o alarme criado pelo canário.
    - Clicar em **Next**.
    - Clicar em **Create health check**.
    - Copiar o ID do health check para ser utilizado na política de tráfego do Route 53.

3. Definir a política de roteamento criando um arquivo politica.json com o seguinte conteúdo:

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

    ![Política de Roteamento](/images/apigw-policy.png)


    ```
    aws route53 create-traffic-policy --name minha_politica --document file://politica.json
    ```

    ```
    aws route53 create-traffic-policy-instance --hosted-zone-id <Id do Hosted Zone criado no passo 2> --name api.example.com --ttl 60 --traffic-policy-id <Id do Traffic Policy criado no passo anterior> --traffic-policy-version 1
    ```

#### Testando o roteamento entre as regiões
1. Uma vez configuradas as entradas de DNS que utiliza a política de failover entre os recursos em diferentes regiões, basta disparar o alarme que indica a falha no health check do ambiente primário que podemos ver que a entrada DNS para o endpoint api.example.com será chaveado para a outra região. 

#### Apagando os recursos

1. Acesse a Console do CloudFormation na região **São Paulo**
2. Selecione a **Stack** criada. 
3. Clique no botão acima **Delete**
4. Confime no botão **Delete stack**
5. Repita os passos 1 ao 4 para a região **N. California** para apagar a segunda **Stack**.