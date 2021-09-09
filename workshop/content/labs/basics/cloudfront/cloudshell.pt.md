---
hidden: true
---


#### 1. Acesse o AWS CloudShell

1. Acesse o AWS CloudShell, na barra superior do AWS Console, clique no botão ao lado direito da barra de busca.

<img src="/images/console-cloudshell2.png?classes=shadow" />

#### 2. Crie as váriaveis de configuração
1. Crie as váriaveis de configuração para facilitar o uso dos scripts
    ```bash
    ACCOUNT_ID=$(aws sts get-caller-identity | jq -r .Account) # Retorna ID da conta AWS
    OHIO_LAB_BUCKET=${ACCOUNT_ID}-mini-lab-cf-ohio # Nome do Bucket para a região primária
    OREGON_LAB_BUCKET=${ACCOUNT_ID}-mini-lab-cf-oregon # Nome do Bucket para a região secundária
    ```
   {{% notice warning %}}
   *Fique atento para não perder a [sessão do AWS CloudShell](https://docs.aws.amazon.com/cloudshell/latest/userguide/limits.html#session-lifecycle-limitations) por inatividade, se a sessão for perdida as variáveis serão removidas.*
   {{% /notice %}}

#### 3. Crie os buckets no Amazon S3 para serem a origem

1. Crie um bucket na região primária, neste caso **Leste dos EUA (Ohio)**.
    ```bash
    aws s3 mb s3://${OHIO_LAB_BUCKET} --region us-east-2
    ```
2. Crie um bucket na região secundária, neste caso **Oeste dos EUA (Oregon)**.
    ```bash
    aws s3 mb s3://${OREGON_LAB_BUCKET} --region us-west-2
    ```
3. Crie os arquivos para enviar aos buckets
   ```bash
   echo "File in Ohio S3" > ohio.index.html
   echo "File in Oregon S3" > oregon.index.html
   ```
4. Copie os arquivos para os respectivos buckets
   ```bash
   aws s3 cp ohio.index.html s3://${OHIO_LAB_BUCKET}/index.html
   aws s3 cp oregon.index.html s3://${OREGON_LAB_BUCKET}/index.html
   ```  

#### 4. Crie um **Origin Access Identity** no Amazon CloudFront 
1. Crie um  **Origin Access Identity** para ter uma identidade e posteriormente ceder o acesso aos buckets de origem.
    ```bash
    OAI_ID=$(aws cloudfront create-cloud-front-origin-access-identity \
             --cloud-front-origin-access-identity-config \
             CallerReference="cloudfront-mini-lab-example",Comment="CloudFront Origin Group Example" \
             | jq -r '.CloudFrontOriginAccessIdentity.Id')
    ```
   {{% notice note %}}
   *É feito o uso de [Command Substitution](https://www.gnu.org/software/bash/manual/html_node/Command-Substitution.html) junto do comando [jq](https://stedolan.github.io/jq/tutorial) para armazenar o resultado do comando em uma váriavel.*
   {{% /notice %}}

  
#### 5. Crie as políticas de acesso nos buckets para permitir o acesso do Amazon CloudFront
1. Crie as políticas de acesso ao bucket na região primária, neste caso **Leste dos EUA (Ohio)**.
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
2. Crie as políticas de acesso ao bucket na região secundária, neste caso **Oeste dos EUA (Oregon)**.
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

3. Aplique as politicas nos buckets
    ```bash
    aws s3api put-bucket-policy \
        --bucket ${OHIO_LAB_BUCKET} --policy file://ohio-s3-policy.json
    aws s3api put-bucket-policy \
        --bucket ${OREGON_LAB_BUCKET} --policy file://oregon-s3-policy.json
    ```

#### 6. Crie a distribuição do Amazon CloudFront

1. Crie o arquivo de configuração da distribuição, esta configuração já criará as duas origens (uma para cada bucket) e o grupo de origem com as duas origens, também desabilita o cache para facilitar os testes.
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
   
2. Crie a distribuição no Amazon CloudFront
    ```bash
    CLOUDFRONT_ID=$(aws cloudfront create-distribution \
                    --distribution-config file://mini-lab-example.json | jq -r '.Distribution.Id')
    ```
   
   {{% notice note %}}
   *É feito o uso de [Command Substitution](https://www.gnu.org/software/bash/manual/html_node/Command-Substitution.html) junto do comando [jq](https://stedolan.github.io/jq/tutorial) para armazenar o resultado do comando em uma váriavel.*
   {{% /notice %}}
   {{% notice note %}}
   *Após solicitar a criação da distribuição é necessário aguardar alguns minutos para que a distribuição esteja disponível.*
   {{% /notice %}}

3. (Opcional) Você pode consultar o status da distribuição com o comando abaixo.
    ```bash
    echo "Status: " $(aws cloudfront get-distribution \
                      --id ${CLOUDFRONT_ID} | jq -r '.Distribution.Status')
    ```
  
#### 7. Teste o Grupo de Origin
1. Pegue a URL da distribuição criada no passo anterior.
    ```bash
    CLOUDFRONT_URL=$(aws cloudfront get-distribution --id ${CLOUDFRONT_ID} | jq -r '.Distribution.DomainName')
    echo ${CLOUDFRONT_URL} 
    ```  
2. Acesse a URL ver a entrega do conteúdo da região primária.
    ```bash
    curl -s ${CLOUDFRONT_URL} # O retorno deverá ser o texto: File in Ohio S3
    ```
3. Remova o arquivo index.html da região primária
    ```bash
    aws s3 rm s3://${OHIO_LAB_BUCKET}/index.html
    ```
4. Acesse novamente a URL da distribuição criada no Amazon CloudFront para ver a entrega sendo feita pela região secundária.
    ```bash
    curl -s ${CLOUDFRONT_URL} # O retorno deverá ser o texto: File in Oregon S3
    ```
5. (Opcional) copie novamente o arquivo para a região primária mas com outro nome
    ```bash
    aws s3 cp ohio.index.html s3://${OHIO_LAB_BUCKET}/ohio.html
    ``` 
6. (Opcional) acesse o arquivo com o novo nome para ver o retorno da região primária
    ```bash
    curl -s ${CLOUDFRONT_URL}/ohio.html # O retorno deverá ser o texto: File in Oregon S3
    ``` 
   

#### 8. Apagando os recursos
1. Desabilite a distribuição usando o recurso de atualizar a configuração da distribuição.
    ```bash
    CLOUDFRONT_DIST=$(aws cloudfront get-distribution-config --id ${CLOUDFRONT_ID}) # Retorna os dados da distribuição
    
    CLOUDFRONT_ETAG=$(echo $CLOUDFRONT_DIST | jq -r '.ETag') # Retorna a ETag da distribuição
   
    echo $CLOUDFRONT_DIST | jq -r '.DistributionConfig' | jq -r .Enabled="false" > mini-lab-example-disabled.json # Retorna a configuração atual alterando o parametro Enabled
   
    CLOUDFRONT_ETAG=$(aws cloudfront update-distribution \
    --id ${CLOUDFRONT_ID} \
    --if-match ${CLOUDFRONT_ETAG} \
    --distribution-config file://mini-lab-example-disabled.json \
    | jq -r .ETag) # Executa o comando para desabilitar o CloudFront e atualizar a variável de ETag
    ```

2. Aguarde alguns minutos até que a distribuição seja removida. Você pode verificar a exclusão já finalizou usando o comando abaixo:
    ```bash
    aws cloudfront get-distribution --id ${CLOUDFRONT_ID} | jq -r .Distribution.Status
    ```
   
3. Remova a distribuição do Amazon CloudFront criada para o laboratório.
    ```bash
    aws cloudfront delete-distribution --id ${CLOUDFRONT_ID} --if-match ${CLOUDFRONT_ETAG}
    ```


4. Apague o Origin Access Identity usado no laboratório.
    ```bash
    OAI_ETAG=$(aws cloudfront get-cloud-front-origin-access-identity --id ${OAI_ID} | jq -r '.ETag')
    
    aws cloudfront delete-cloud-front-origin-access-identity \
    --id ${OAI_ID} --if-match ${OAI_ETAG}
    ```
5. Apague os buckets usados no laboratório, nas duas regiões.
    ```bash
    aws s3 rb s3://${OHIO_LAB_BUCKET} --force
    aws s3 rb s3://${OREGON_LAB_BUCKET} --force
    ```
6. Apague os arquivos criados para o labortório.
    ```bash
    rm ohio.index.html \
       oregon.index.html \
       ohio-s3-policy.json \
       oregon-s3-policy.json \
       mini-lab-example.json \
       mini-lab-example-disabled.json
    ```