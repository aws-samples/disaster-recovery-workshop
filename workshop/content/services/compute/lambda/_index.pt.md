---
title: "AWS Lambda"
menutitle: "AWS Lambda"
weight: 1
chapter: true
pre: "<b></b>"
---

O AWS Lambda é um serviço de computação que permite que você execute o código sem provisionar ou gerenciar servidores. O AWS Lambda executa o código somente quando necessário e dimensiona automaticamente, desde algumas solicitações por dia a milhares por segundo. Você paga somente pelo tempo de computação utilizado; não haverá cobrança quando seu código não estiver em execução. 

{{% notice tip %}}
O modelo de custo de aplicações Serverless utilizando o AWS Lambda é uma grande vantagem em cenários de *Disaster Recovery* visto que você paga apenas pela utilização, e não pela disponibilidade dos recursos alocados.
{{% /notice %}}

Com o AWS Lambda, você pode executar códigos para qualquer tipo de aplicativo ou serviço de back-end praticamente, todos sem administração. As seguintes [linguagens são compatíveis com o AWS Lambda](https://docs.aws.amazon.com/pt_br/lambda/latest/dg/lambda-runtimes.html).

#### Gerenciamento de Lambdas multi-region
Utilize ferramentas de versionamento para gerenciar suas funções AWS Lambda como o [CodeCommit](https://aws.amazon.com/codecommit/) por exemplo. Não é o foco desta página abordar o tema, mas também é possível armazenar o seu código fonte em múltiplas regiões. Este blog post [blog post](https://aws.amazon.com/blogs/devops/replicate-aws-codecommit-repository-between-regions-using-aws-fargate/) descreve uma possível arquitetura para isto.

As estratégias de *Disaster Recovery* necessitam que suas funções AWS Lambda sejam instaladas em mais de uma região. Não há uma forma automática para isto, visto que as regiões são totalmente independentes. Abaixo, descrevemos as possibilidades para cada abordagem:


##### Backup & Restore
Consiste em manter um backup de suas funções AWS Lambda para que seja possível restaurá-las em outra região em caso de disastre. Mantenha o código fonte e as configurações de suas funções sob gestão de uma ferramenta de versionamento e utilize tags para fazer o baseline de suas versões de código. Durante o processo de restore, é possível instalar suas funções de duas formas: [Manualmente]({{< ref "manual" >}} "Manualmente") ou através de um [Pipeline de CI/CD]({{< ref "pipeline" >}} "Pipeline de CI/CD"). 

##### Pilot Light
Para esta estratégia, é preciso que as funções AWS Lambda que são críticas para o funcionamento mínimo da região de DR estejam funcionais e atualizadas. Desta forma, considere minimamente que as funções AWS Lambda críticas sejam gerenciadas via [Pipeline de CI/CD]({{< ref "pipeline" >}} "Pipeline de CI/CD"). No momento do provisionamento da infra de produção em caso de desastre, provisione o ambiente também através dos [Pipelines de CI/CD]({{< ref "pipeline" >}} "Pipeline de CI/CD") ou, minimamente, [de forma manual]({{< ref "manual" >}} "Manualmente").


##### Warm Standby & Active-Active
Neste caso, você terá dois ambientes produtivos funcionais, porém com um deles rodando na capacidade mínima. Manter ambos manualmente é inviável, e neste caso o [Pipeline de CI/CD]({{< ref "pipeline" >}}) é a opção a ser utilizada.

---
**Referências**
- [AWS Lambda](https://docs.aws.amazon.com/pt_br/lambda/latest/dg/welcome.html)
- [Replicate AWS CodeCommit Repositories between Regions using AWS Fargate](https://aws.amazon.com/blogs/devops/replicate-aws-codecommit-repository-between-regions-using-aws-fargate/)