---
title: "AWS Backup - Plano de Backup Automático"
menutitle: "Plano de Backup Automático"
date: 2020-04-24T11:16:08-04:00
chapter: false
weight: 2
tags:
  - Backup & Restore
type: Lab
awsServices:
  - Amazon EBS

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

**Fonte:** [AWS Well-Architected Labs: Testing Backup and Restore of Data](https://wellarchitectedlabs.com/reliability/200_labs/200_testing_backup_and_restore_of_data/)


<!-- TODO: Incluir estimativa de custos!!! --->

