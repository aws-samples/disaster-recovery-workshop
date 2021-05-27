---
title: "Amazon EC2 - Teste de Resiliência"
menutitle: "EC2 - Teste de Resiliência"
chapter: false
weight: 2
tags:
 - Pilot Light
type: Lab
awsServices:
 - Amazon EC2

draft: true

---

### Objetivo

Esse exercício irá mostrar os passos para a criação de um plano de backup para as máquinas Amazon EC2.

**Ao final desse exercício, você será capaz de:**
- Criar uma estratégia de backup
- Testar a restauração a partir dos backups
- Automatizar o processo de backup e restauração


### Visão Geral da Solução
![Backup Plan Automation](/images/auto-backup-plan.jpeg)

### Execução
{{< tabs name="labs_types" >}} 
{{< tab name="AWS Console" include="console" />}}  
{{< /tabs >}}



### Conclusão

Com esse exercício foi possível verificar como tabelas replicadas do Amazon DynamoDB podem ser utilizadas em uma estratégia do tipo *Active-Active*, tanto para leitura quando para persistir dados.  


#### Autor

* Seth Eliot, Principal Reliability Solutions Architect, AWS Well-Architected


**Fonte:** [AWS Well-Architected Labs: Testing for EC2 Instances](https://wellarchitectedlabs.com/reliability/200_labs/200_testing_for_resiliency_of_ec2/)


<!-- TODO: Incluir estimativa de custos!!! --->
