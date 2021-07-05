---
title: "AWS Secrets Manager - Replicação de Segredos"
menutitle: "AWS Secrets Manager - Replicação de Segredos"
weight: 1
pre: "<b></b>"
hidden: true
---

#### Crie um Segredo

1. Acesse o serviço do AWS Secrets Manager: [https://console.aws.amazon.com/secretsmanager/](https://console.aws.amazon.com/secretsmanager/home?region=us-east-1)
2. Clique em **Store a new secret**
3. Ná tela **Store a new secret**, selecione **Other type of secrets**. Esse tipo de segredo é usado quando não se aplica a um banco de dados.
4. Na seção **Specify the key/value pairs to be stored in this secret**, no primeiro campo digite **MyFirstSecret**, digite seu segredo no segundo campo.![Secret - Create new secret](/images/secretmanager-create.png)
5. Clique em **Next**
6. Digite o nome do segredo no campo **Secret name**
7. Clique em **Next**
8. Na etapa 3, clique em **Next**
9.  Na etapa 4 - **Review**, clique em **Next**

#### Replicando o Segredo

1. Localize o Segredo criado em **AWS Secrets Manager > Secrets**
![looking](/images/secretmanager-looking.png)

2. Clique no Segredo que deseja replicar para outra região

3. Na aba do Segredo selecionado, clique em **Replicate secret to other regions**
![accessing](/images/secretmanager-accessing.png)

4. Na aba **Add replica regions**, configure detalhes da replicação. No campo **AWS Region** selecione a região **Ohio**
![replicating](/images/secretmanager-replicating.png)
5. Clique em **Complete adding region(s)** {{% notice tip %}}
É possível adicionar mais de uma região para replicação, clicando em **Add more regions**
{{% /notice %}}


Pronto! Você acabou de replicar seu Segredo para outra região AWS.

#### Visualizando o resultado

1. Selecione a região para qual o Segredo foi replicado **Ohio**

2. Localize o Segredo replicado em **AWS Secrets Manger > Secrets**
![Secret Manager Replication](/images/secretmanager-replication-result.png)



{{% notice note %}}
O Segredo replicado está associado com o Segredo primário, para deixa-lo independente basta clicar em **Promote to standalone secret**
{{% /notice %}}

#### Apagando os recursos
1. Acesse o serviço do AWS Secrets Manager na região primária **N. Virginia**: [https://console.aws.amazon.com/secretsmanager/](https://console.aws.amazon.com/secretsmanager/home?region=us-east-1)
2. Clique na chave criada. 
3. Na seção **Replicate Secret**, selecione **Actions > Delete Replica** ![Delete Replica](/images/secretmanager-deletereplica.png)
4. Na tela **Delete Replica** confirme digitando a região na qual a replica deve ser apagada **us-east-2** e clique em **Delete Replica**.
5. Vá até o início da página e clique em **Action > Delete Secret**. 
6. Na tela **Disable secret and schedule deletion**, confirme clicando em **Schedule deletion**





