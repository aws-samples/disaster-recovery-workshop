---
title: "Amazon FSx for Windows File Server - Replicação de dados entre regiões"
menutitle: "FSx entre Regiões AWS"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Pilot Light]
type: Lab
awsServices:
 - Amazon FSx
---

TODO: remove this!!!
Senha123456


### Introdução
Para os clientes que utilizam o Amazon FSx como local de armazenamento e que possuem requisitos de conformidade ou recuperação de desastres, é necessário a replicação dos dados do servidor de arquivos para outras regiões da AWS. Apesar que o Amazon FSx já possui mecanismos de resiliência embarcados entre zonas de disponibilidade, a replicação pode proteger os clientes em um cenário de indisponibilidade de uma região.


### Visão Geral da Solução


![any](/images/fsx-win-architecture.png)

### Objetivos

Aprender como configurar a replicação do Amazon FSx for Windows File Server entre regiões AWS usando o AWS DataSync. Esse processo envolve os seguintes passos:

* Configurar do Amazon FSx na região primária.
* Habilitar a comunicação entre as regiões com VPC Peering.
* Configurar o Amazon FSx na região secundária. 
* Configurar o agente do AWS DataSync 
* Configurar o serviço do AWS DataSync para replicar os dados da região primária para a região secundária.



**Ao final desse exercício, você será capaz de:**
- Replicar dados do Amazon FSx entre regiões AWS.

**Tempo de Execução Estimado:** 1 hora e 30 minutos

### Execução
{{< tabs name="labs_types" >}} 
{{< tab name="AWS Console" include="console" />}} 
{{< /tabs >}}

<!--
### Congratulations!

You have set up AWS DataSync to copy files from the Amazon FSx file system in the source AWS Region to the Amazon FSx file system in the DR AWS Region. During task execution, AWS DataSync examines the source files and only copies the files that have changed. If you would like to watch a walkthrough of the process, please review this 
{{< youtube YO-i6GIGD1E>}}

*(you can skip to 3:37 in the video)*.
-->


### Conclusão

Nesse exercício, você aprendeu como criar uma cópia de um volume do Amazon FSx para outra região AWS, habilitando assim, uma mudança de região para recuperação de desastres. Foi mostrado como o AWS DataSync pode ser usado de maneira contínua para replicar dados entre regiões. O AWS DataSync automatiza as tarefas de replicação de forma eficiente replicando apenas os dados modificados.


----- 

**Fonte:** [Amazon FSx - How to replicate Amazon FSx for Windows File Server data across AWS Regions](https://aws.amazon.com/blogs/storage/how-to-replicate-amazon-fsx-file-server-data-across-aws-regions/)




