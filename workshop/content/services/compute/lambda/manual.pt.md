---
title: "AWS Lambda - Movimentação Manual"
menutitle: "Movimentação Manual"
weight: 4
awsServices: 
 - AWS Lambda
tags:
 - Backup & Restore
 - Pilot Light
---

Envolve exportar manualmente, via console ou aws-cli, suas funções lambda e importá-las em outra região. 
{{% notice warning %}}
Apesar de possível, esta estratégia é muito suscetível a erros manuais. Utilizar quando se tem poucas funções pode ser uma estratégia, embora desencorajada. Apenas o código fonte será exportado, fazendo com que as demais configurações como Permissões, memória, timeout, variáveis de ambiente e demais opções tenham que ser configuradas manualmente. Além disto, mantenha sempre seu código fonte em uma ferramenta de versionamento.
{{% /notice %}}

Caso você tenha o código fonte armazenado em um gerenciador de versão, O passo de exportação abaixo não é necessário.

#### Exportação (via Console)
Para isto, acesse o menu Serviços -> Lambda. Selecione Functions, clique na função desejada, actions -> Função de Exportação. Um popup irá aparecer com as opções de Download do Arquivo SAM ou do Pacote de implantação. Selecione Pacote de Implantação e o download será feito. Repare que o arquivo baixado é um arquivo zip, porém não tem esta extensão. Após o download, renomeie o arquivo para a extensão .zip para ser utilizado posteriormente.

#### Exportação (via CLI)
AWS CLI fornece comandos para para listar e baixar localmente todas as funções AWS Lambda. Por exemplo, para listar o nome das funções existentes na sua conta configurada em `~/.aws/credentials`:

```
aws lambda list-functions --region=us-east-2 | jq '[.Functions[] | select(.FunctionName) | .FunctionName]'
```

O comando acima extrai apenas o nome da função via jq. Para obter os detalhes de uma função lambda específica pelo nome:

```
aws lambda get-function --region=us-east-2 --function-name test | jq
```

O comando acima retorna inclusive a url S3 de onde o código fonte está localizado no node ``Code.Location`` do json retornado, além das configurações de memória, timeout e outos em ``Configuration``. 

#### Importação (via Console)
Volte para a lista de Funções no breadcrumb no canto superior esquerdo. Selecione a nova região no combo no canto superior direito do Console e clique em Criar função (caso ela não exista). Nomeie a função preferencialmente com o mesmo nome da região, tomando cuidado para garantir que as demais configurações como runtime, permissões, mémória e demais opções estejam iguais a da região de origem. Uma vez criada, na seção Código Fonte:
* Clique em Ações -> Fazer upload de um arquivo zip. 
* Selecione o arquivo exportado previamente
* Clique em ok. 
 
O código fonte será importado para a função Lambda corrente. Você pode também importar o código fonte à partir de um .zip armazenado em um bucket S3. 

#### Importação (via CLI)
Também é possível criar funções AWS Lambda utilizando o AWS CLI. Para isto, utilize o método abaixo:

```
aws lambda create-function --function-name funcao-teste --zip-file fileb://meufonte.zip --handler meuhandler --runtime meuruntime --role arn:aws:iam:meuaccountid:role/minharole
``` 

O comando acima é apenas um exemplo. Substitua os valores ``funcao-teste, meufonte.zip, meuhandler, meuruntime, meuaccountid e minharole`` pelos valores específicos ao seu cenário. A documentação completa encontra-se [aqui](https://docs.aws.amazon.com/pt_br/lambda/latest/dg/gettingstarted-awscli.html).

#### Considerações
Os comandos do AWS CLI listados acima são apenas exemplos. É possível automatizar parte desta importação e exportação via shell script ou python, por exemplo, para minimizar possíveis problemas de configuração fazendo-as manualmente pelo Console. Elabore a estratégia mais segura para o seu cenário e evolua sua arquitetura sempre que possível e baseando-se nos [Pilares para workloads resilientes]({{< ref "intro/disaster-recovery/architecture-pillars" >}} "Pilares para workloads resilientes").

{{% notice info %}}
Considere minimamente encapsular seu código fonte utilizando o [SAM (Serverless Application Model)](https://aws.amazon.com/serverless/sam/). Desta forma, é possível transportar o código fonte, suas configurações e dependências em um único pacote. Isto simplifica drásticamente a complexidade de movimentação dos seus pacotes de instalação entre regiões e segue o pilar de [Infrastructure-as-code]({{< ref "intro/disaster-recovery/architecture-pillars" >}} "Infrastructure-as-code") descrito neste material.
{{% /notice %}}

Além do SAM descrito acima, é possível também utilizar ferramentas como o AWS [CloudFormation](https://aws.amazon.com/cloudformation/) ou o [AWS CDK](https://aws.amazon.com/cdk/) via linha de comando. Ainda que executado manualmente, minimamente estas ferramentas garantem um *diff* de sua infraestrutura atual, facilitando o *deployment* das evoluções de sua arquitetura nas múltiplas regiões.

---
**Referências**
- [AWS CloudFormation](https://aws.amazon.com/pt/cloudformation/)
- [Adicionar uma ação entre regiões no CodePipeline](https://docs.aws.amazon.com/pt_br/codepipeline/latest/userguide/actions-create-cross-region.html)
- [Deploy code in multiple AWS Regions using AWS CodePipeline, AWS CodeCommit, and AWS CodeBuild](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/deploy-code-in-multiple-aws-regions-using-aws-codepipeline-aws-codecommit-and-aws-codebuild.html)
- [Pilares para workloads resilientes]({{< ref "intro/disaster-recovery/architecture-pillars" >}} "Pilares para workloads resilientes")
- [AWS CLI Docs - Lambda](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-awscli.html)