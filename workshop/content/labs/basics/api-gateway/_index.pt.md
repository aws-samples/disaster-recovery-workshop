---
title: "Amazon API Gateway + Route53"
menutitle: "Amazon API Gateway + Route53"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Active-Passive]
type: Lab
awsServices:
 - Amazon API Gateway
---

### Objetivo

Esse exercício irá mostrar os passos para você configurar rapidamente suas apis em mais de uma região da AWS e permitir que se comportem de forma ativo-passivo recebendo requisições de forma transparente dos seus clientes. 

**Conhecimentos necessários:** 
- Uso básico do AWS CLI
- Conceitos básicos sobre balanceadores de carga


**Ao final desse exercício, você será capaz de:**

- Configurar uma api em duas ou mais regiões
- Configurar um mecanismo de DNS para fazer o roteamento dinâmico para suas apis de forma transparente para seus clientes


**Tempo de Execução Estimado:** 40 minutos

**Custo Aproximado**: 3 USD
In you diagram and in the CFN template both vpc have CIDR 10.0.0.0/16. With this configuration VPC peering will not work as overlapping VPC CIDR ranges are not allowed. 
![Arquitetura API Gateway](/images/apigw-lab-architecture.png)

### Execução

{{< tabs name="labs_types" >}}  
{{< tab name="AWS CloudShell" include="cloudshell" />}}  
{{< /tabs >}}

### Conclusão

Com esse exercício, foi possivel entender como configurar uma api privada no Amazon API Gateway, fazer o deploy da API em mais de uma região e criar uma política de roteamento para seus registros de DNS associados aos endpoints de sua API.

[Alternativa para Custom Domain Name](https://georgemao.medium.com/enabling-private-apis-with-custom-domain-names-aws-api-gateway-df1b62b0ba7c)
