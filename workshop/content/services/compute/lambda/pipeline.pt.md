---
title: "AWS Lambda - Pipeline de CI/CD"
menuTitle: "Pipeline de CI/CD"
weight: 5
awsServices: 
 - AWS Lambda
tags:
 - Warm Standby
---

Consiste em configurar seu Pipeline de entrega de código fazendo a instalação via CodePipeline e CodeDeploy das funções AWS Lambda em mais de uma região. É possível fazer a instalação em mais de uma região ao mesmo tempo, ou então parametrizar seu Pipeline informando a região a ser feita a instalação no momento da execução do Pipeline. O CodePipeline suporta nativamente [ações Cross-Region](https://docs.aws.amazon.com/codepipeline/latest/userguide/actions-create-cross-region.html).
{{% notice tip %}}
Esta é a abordagem recomendada. Desta forma a versão das suas funções AWS Lambda estarão sincronizadas por utilizarem a mesma origem de armazenamento através de uma ferramenta de versionamento.
{{% /notice %}}

Esta estratégia traz mais visibilidade sobre o estado das instalações, possibilitando que estes *deployments* em cada região sejam rastreáveis. Descreva suas funções AWS Lambda via [SAM (Serverless Application Model)](https://aws.amazon.com/serverless/sam/) para garantir que os parâmetros como permissões, memória, timeout, variáveis de ambiente e demais opções estejam sincronizadas. Este [blog post](https://aws.amazon.com/blogs/devops/using-aws-codepipeline-to-perform-multi-region-deployments/) descreve como utilizar o CodePipeline e CodeDeploy para instalar artefatos multi-region. [Neste padrão](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/deploy-code-in-multiple-aws-regions-using-aws-codepipeline-aws-codecommit-and-aws-codebuild.html) do [Prescriptive Guidance para DevOps](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/devops-pattern-list.html), demonstramos como automatizar o deployment multi-region utilizando CloudFormation, que contém o seu código AWS Lambda, suas configurações e dependências necessárias de maneira unificada.


---
**Referências**
- [Using AWS CodePipeline to Perform Multi-Region Deployments](https://aws.amazon.com/blogs/devops/using-aws-codepipeline-to-perform-multi-region-deployments/)
- [Add a cross-region action in CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/actions-create-cross-region.html)
- [Deploy code in multiple AWS Regions using AWS CodePipeline, AWS CodeCommit, and AWS CodeBuild](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/deploy-code-in-multiple-aws-regions-using-aws-codepipeline-aws-codecommit-and-aws-codebuild.html)