---
hidden: true
type: lab
---


#### Acesse o AWS CloudShell

1. Acesse o AWS ClouShell, na barra superior do AWS Console, clique no botão ao lado direito da barra de busca.

<img src="/images/console-cloudshell2.png?classes=shadow" />


#### Crie uma tabela do Amazon DynamoDB

1. Crie uma nova tabela (**Music**) na região **Leste dos EUA (Ohio)**.
    ```bash
        aws dynamodb create-table \
            --table-name Music \
            --attribute-definitions \
                AttributeName=Artist,AttributeType=S \
                AttributeName=Song,AttributeType=S \
            --key-schema \
                AttributeName=Artist,KeyType=HASH \
                AttributeName=Song,KeyType=RANGE \
            --billing-mode PAY_PER_REQUEST \
            --stream-specification StreamEnabled=true,StreamViewType=NEW_AND_OLD_IMAGES \
            --region us-east-2
    ```

#### Crie uma réplica da tabela em outra região AWS
1. Crie uma tabela **Music** idêntica na **Oeste dos EUA (Oregon)**.
    ```bash
    aws dynamodb update-table --table-name Music --cli-input-json --region us-east-2 \
        '{
            "ReplicaUpdates":
            [
                {
                "Create": {
                    "RegionName": "us-west-2"
                    }
                }
            ]
        }' 
    ```
2. Repita a etapa anterior para criar uma réplica tabela em terceira região. **Europa (Irlanda) (eu-west-1)**.
    ```bash
    aws dynamodb update-table --table-name Music --cli-input-json --region us-east-2 \
        '{
            "ReplicaUpdates":
            [
                {
                "Create": {
                    "RegionName": "eu-west-1"
                    }
                }
            ]
        }' 
    ```

3. (Opcional) É possível visualizar a lista de réplicas criadas usando describe-table.
    ```bash
    aws dynamodb describe-table --table-name Music --region us-east-2
    ```

{{% notice note %}}
*Caso alguma das tabelas não esteja na lista de réplicas, aguarde alguns minutos para que sejam criadas.*
{{% /notice %}}

    

#### Insira dados de exemplo

1. Para verificar se a replicação está funcionando, adicione um novo item à tabela Music na região **Leste dos EUA (Ohio)**.
    ```bash
    aws dynamodb put-item \
        --table-name Music \
        --item '{"Artist": {"S":"The Beatles"},"Song": {"S":"Hey Jude"}}' \
        --region us-east-2
    ```

2. Para verificar se a replicação está funcionando, adicione um novo item à tabela Music na região **Leste dos EUA (Ohio)**.
    ```bash
    aws dynamodb put-item \
        --table-name Music \
        --item '{"Artist": {"S":"The Clash"},"Song": {"S":"London Calling"}}' \
        --region us-west-2
    ```

3. Verifique se os itens foram replicados com êxito para a região **Europa (Irlanda)**:    
    ```bash
    aws dynamodb scan --table-name Music --region eu-west-1
    ```


#### Apagando os recursos
1. Exclua a tabela-réplica na região **Europa (Irlanda)** e região **Oeste dos EUA (Oregon)**.

    ```bash
    aws dynamodb update-table --table-name Music --cli-input-json --region us-east-2 \
    '{
        "ReplicaUpdates":
        [
            {
            "Delete": {
                "RegionName": "us-west-2"
                },
            "Delete": {
                "RegionName": "eu-west-1"
                }
            }
        ]
    }'
    ```

2. Aguarde alguns minutos até que as duas tabelas sejam apagadas. Você pode verificar a exclusão já finalizou usando o comando abaixo:
    ```bash
    aws dynamodb describe-table --table-name Music --region us-east-2
    ```

3. Apague a tabela **Music** na região **Leste dos EUA (Ohio)**.
    ```bash
    aws dynamodb delete-table --table-name Music --region us-east-2
    ```