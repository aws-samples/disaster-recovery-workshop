---
title: "Amazon DynamoDB - Tabelas Globais"
menutitle: "DynamoDB - Tabelas Globais"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Active-Active]
type: Lab
awsServices:
 - Amazon DynamoDB
---

### Objetivo

Esse exercício irá mostrar os passos para o uso do mecanismo Global Tables para tabelas do Amazon DynamoDB. Caso queira mais informações [clique aqui]({{< ref "/services/databases/dynamodb/dynamo-global-table" >}} "Amazon DynamoDB").

**Ao final desse exercício, você será capaz de:**
- Criar tabelas replicadas do Amazon DynamoDB em outra regiões da AWS

**Tempo de Execução Estimado:** 20 minutos

**Custo Aproximado**: 1 USD

### Execução
{{< tabs name="labs_types" >}} 
{{< tab name="AWS Console" include="console" />}} 
{{< tab name="AWS CloudShell" include="cloudshell" />}}  
{{< /tabs >}}

### Conclusão

Com esse exercício foi possível verificar como tabelas replicadas do Amazon DynamoDB podem ser utilizadas em uma estratégia do tipo *Active-Active*, tanto para leitura quando para persistir dados.  

**Fonte:** [Amazon DynamoDB - Guia do Desenvolvedor](https://docs.aws.amazon.com/pt_br/amazondynamodb/latest/developerguide/V2globaltables.tutorial.html)


<!-- TODO: Incluir estimativa de custos!!! --->

