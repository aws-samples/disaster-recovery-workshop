---
title: "AWS Lambda - Exportação das funções para outras regiões"
menutitle: "Lambda - Exportação das funções"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Backup-Restore, Pilot Light]
type: Lab
awsServices:
 - AWS Lambda
---

### Objetivo

Esse exercício irá mostrar os passos para exportar e importar funções Lambda entre regiões manualmente. Caso queira mais informações [clique aqui]({{< ref "/services/compute/lambda/" >}} "AWS Lambda").

**Conhecimentos necessários:** 
- Uso básico do AWS CLI
- Conceitos básicos da linguagem Javascript


**Ao final desse exercício, você será capaz de:**
- Exportar e importar funções Lambda entre regiões

**Tempo de Execução Estimado:** 30 minutos

**Custo Aproximado**: 1 USD


### Execução
{{< tabs name="labs_types" >}} 
{{< tab name="AWS Console" include="console" />}} 
{{< tab name="AWS CloudShell" include="cloudshell" />}}  
{{< /tabs >}}

### Conclusão

Com este exercício foi possível verificar como a replicação de funções Lambda entre regiões AWS pode ser utilizada nas estratégias de DR do tipo *Backup & Restore* e *Pilot Light*. Para as estratégias de *Warm Standby* e *Active-Active*, consulte o exercício prático de deployment de funções lambda via Pipeline de CI/CD.

**Fonte:** [AWS Lambda - Guia do Desenvolvedor](https://docs.aws.amazon.com/pt_br/lambda/latest/dg/gettingstarted-awscli.html)


<!-- TODO: Incluir estimativa de custos!!! --->

