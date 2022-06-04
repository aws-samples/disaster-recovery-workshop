---
title: "Amazon Route 53 - Política de Failover em Private Hosted Zone"
menutitle: "Route 53 - Política de Failover em Private Hosted Zone"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Active-Active]
type: Lab
awsServices:
 - Amazon Route 53
draft: true
---

### Objetivo

Esse exercício irá mostrar os passos para você configurar rapidamente a funcionalidade de failover baseado em DNS para endpoints existentes em diferentes regiões. 

Aviso: para este laboratório, você deve usar as regiões US-EAST-1 e US-WEST-1. Se você estiver executando um evento da AWS, confirme essas informações com seu instrutor.

**Conhecimentos necessários:** 
- Uso básico do AWS CLI
- Conceitos básicos sobre balanceadores de carga


**Ao final desse exercício, você será capaz de:**

- Configurar uma política de failover no Amazon Route 53 para permitir o chaveamento da entrada DNS para recursos em regiões diferentes.
- Arquitetar um mecanismo de failover para recursos privados na sua VPC.

**Tempo de Execução Estimado:** 40 minutos

**Custo Aproximado**: 3 USD

![Arquitetura Route 53](/images/route53-lab-architecture.png)

### Execução
{{< tabs name="labs_types" >}}  
{{< tab name="AWS CloudShell" include="cloudshell" />}}  
{{< /tabs >}}

### Conclusão

Com esse exercício, foi possivel entender como configurar entradas de DNS no Route 53 para fazer o roteamento dinâmico entre recursos em diferentes regiões.
Para contextos mais complexos envolvendo on-premises e ambiente multi-account, a seguinte referência apresenta um arquitetura de exemplo que pode ser aplicada.

- [Multi region DNS support](https://aws.amazon.com/blogs/architecture/using-route-53-private-hosted-zones-for-cross-account-multi-region-architectures/)

- [Route53 healthchecks for Private Hosted Zone](https://aws.amazon.com/blogs/networking-and-content-delivery/performing-route-53-health-checks-on-private-resources-in-a-vpc-with-aws-lambda-and-amazon-cloudwatch/)
