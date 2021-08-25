---
title: "Amazon CloudFront - Grupo de Origem"
menutitle: "CloudFront - Grupo de Origem"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Warm Standby]
type: Lab
awsServices:
 - Amazon CloudFront
---

### Objetivo

Esse exercício irá mostrar os passos para o uso do mecanismo Grupo de Origem do Amazon CloudFront, utilizando buckets do Amazon S3 como origem. Caso queira mais informações [clique aqui]({{< ref "/services/networking/cloudfront" >}} "Amazon CloudFront").

**Ao final desse exercício, você será capaz de:**
- Criar distribuições do CloudFront resilientes com duas origens, uma primária e uma secundária para failover.

**Tempo de Execução Estimado:** 30 minutos

**Custo Aproximado**: 1 USD

### Execução
{{< tabs name="labs_types" >}} 
{{< tab name="AWS CloudShell" include="cloudshell" />}}  
{{< /tabs >}}

### Conclusão

Com esse exercício foi possível verificar como as distribuições do Amazon CloudFront podem ser resilientes usando duas ou mais origens com o recurso de Grupo de Origem em uma estratégia do tipo *Active-Passive*.  

**Fonte:** [Amazon CloudFront - Guia do desenvolvedor](https://docs.aws.amazon.com/pt_br/AmazonCloudFront/latest/DeveloperGuide/high_availability_origin_failover.html#concept_origin_groups.creating)

