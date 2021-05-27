---
title: "Amazon ECR - Replicação do Registro de Imagens"
menutitle: "Amazon ECR - Replicação do Registro de Imagens"
weight: 1
pre: "<b></b>"
hidden: true
---

{{% notice info %}}
Caso ainda não tenha criado o ambiente no AWS Cloud9, comece criando seu workspace. [Clique aqui](pt/../../../../prereqs/workspace/workspace).
{{% /notice %}}

{{% notice note %}}
Para esse exercício utilize **N. Virginia (us-east-1)** como a região principal e a **N. California (us-west-1)** como a região secundária. 
{{% /notice %}}

#### Criar um Repositório no Registro  

1. Salve o ID da sua conta AWS em uma variável de ambiente.
    ```bash
    sudo yum install jq -y
    export AWSACCOUNT=$(aws sts get-caller-identity | jq -r '.Account')
    ```

1. Crie um repositório no *registry* privado. Obs: Toda AWS Account já possui um *registry* privado.  

    ```bash
    aws ecr create-repository \
        --repository-name ecr-repository --region us-east-1
    ```

#### Criar uma imagem e fazer push para o Repositorio criado  

1. Crie o index.html  

    ```bash

    cat > index.html << EOF
        <!doctype html>
        <html lang="en">
            <head>
                <meta charset="utf-8">
                <title>Docker Nginx</title>
            </head>
            <body>
                <h2>Hello from Nginx container</h2>
            </body>
        </html>
    EOF

    ```

2. Crie o Dockerfile  

    ```bash
    cat > Dockerfile << EOF
    FROM nginx:latest
    COPY ./index.html /usr/share/nginx/html/index.html
    EOF
    ```

3. Crie a imagem docker com o nome do repositorio (substitua o id da conta conforme o seu ambiente)

    ```bash

    docker build -t $AWSACCOUNT.dkr.ecr.us-east-1.amazonaws.com/ecr-repository:webapp .

    ```

4. Para fazer push da imagem para o repositório utilizando o docker, é necessário se autenticar no repositório primeiro. Para isso, utilize o comando get-login e obtenha a senha para autenticar o docker com o comando docker login. Para o comando abaixo, substitua o id da conta conforme o seu ambiente.

    ```bash

    aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin $AWSACCOUNT.dkr.ecr.us-east-1.amazonaws.com

    ```

5. Depois de autenticar, basta fazer o push usando o comando docker push e substituindo o id da conta conforme o seu ambiente.

    ```bash

    docker push $AWSACCOUNT.dkr.ecr.us-east-1.amazonaws.com/ecr-repository:webapp
        
    ```

#### Configurar replicação para outra região no registro de imagens

1. Obtenha o id do registro.  

    ```bash

    export REGISTRYID=$(aws ecr describe-registry | jq -r '.registryId')

    ```

2. Criar um arquivo json com os parametros de configuração. Substitua o id de registro do seu ambiente. Perceba que a configuração de replicação irá replicar para a região de **N. California (us-west-1)**.  

    ```bash

    cat > replication.json << EOF
    { 
        "rules": [ 
            { 
                "destinations": [ 
                    {
                        "region": "us-west-1", 
                        "registryId": "$REGISTRYID" 
                    } 
                ] 
            } 
        ] 
    } 
    EOF

    ```

3. Habilite a replicação com o comando abaixo.  

    ```bash
    aws ecr put-replication-configuration \
    --replication-configuration file://replication.json --region us-east-1
    ```

#### Faça uma atualização na imagem  

1. Atualize a imagem do container gerando uma nova versão com o comando a seguir. Substitua o id da conta conforme o seu ambiente.  

    ```bash

    docker build -t $AWSACCOUNT.dkr.ecr.us-east-1.amazonaws.com/ecr-repository:webapp2 .

    ```

2. Faça push da nova versão. Perceba que o push acontece para a região de origem (nesse caso N.Viginia (us-east-1)).  

    ```bash

    docker push $AWSACCOUNT.dkr.ecr.us-east-1.amazonaws.com/ecr-repository:webapp2

    ```

#### Valide que a imagem está em ambas as regiões

1. Liste os repositorios de imagens de ambas as regiões. No retorno do comando, observe o parametro *repositoryUri*.  

    ```bash

    aws ecr describe-repositories --region us-east-1
    aws ecr describe-repositories --region us-west-1

    ```

2. Liste as imagens de ambas as regiões. Observe que as imagens possuem o mesmo tamanho, o mesmo manifesto. Porém, a data de push no repositório é um pouco diferente (parametro imagePushedAt).

    ```bash
    aws ecr describe-images --repository-name ecr-repository --region us-east-1
    aws ecr describe-images --repository-name ecr-repository --region us-west-1
    ```

#### Apagando os recursos


1. Apague as imagens criadas nos repositórios
    ```bash
    aws ecr batch-delete-image \
    --repository-name ecr-repository \
    --image-ids imageTag=webapp imageTag=webapp2 --region us-east-1
    aws ecr batch-delete-image \
    --repository-name ecr-repository \
    --image-ids imageTag=webapp imageTag=webapp2 --region us-west-1
    ```

2. Apague os repositórios criados
    ```bash
    aws ecr delete-repository --repository-name ecr-repository --region us-east-1
    aws ecr delete-repository --repository-name ecr-repository --region us-west-1
    ```
