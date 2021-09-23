---
title: "Amazon ECR - Replicação do Registro de Imagens"
menutitle: "ECR - Replicação do Registro de Imagens"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Active-Active]
type: Lab
awsServices:
 - Amazon ECR
draft: true
---

### Objetivo

Esse exercício irá mostrar os passos para habilitar a replicação de registro no Amazon Elastic Container Registry. Em um cenário de multi região, as imagens de container armazenadas precisam estar disponíveis em uma região secundária para que a apliacação possa ser configurada durante o desastre.

**Ao final desse exercício, você será capaz de:**

- Configurar a replicação para outra região de um registro de imagens do Amazon ECR
- Listar imagens e verificar que a replicação está funcionando de acordo com a configuração aplicada.

**Tempo de Execução Estimado:** 20 minutos

**Custo Aproximado**: 1 USD

### Execução
{{< tabs name="labs_types" >}}  
{{< tab name="AWS Cloud9" include="cloud9" />}}
{{< /tabs >}}

### Conclusão

Com esse exercício, foi possivel entender como configurar um repositório de imagens do Amazon Elastic Container Registry, fazer o push de uma imagem para o repositório, configurar a replicação com uma segunda região, atualizar a imagem e verificar que a replicação da imagem foi realizada com sucesso para a segunda região.   

**Fonte:** [[Conteúdo em Inglês] Cross region replication in Amazon ECR has landed](https://aws.amazon.com/pt/blogs/containers/cross-region-replication-in-amazon-ecr-has-landed/)
