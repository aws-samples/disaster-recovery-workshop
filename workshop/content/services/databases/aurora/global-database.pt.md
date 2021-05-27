---
title: "Aurora - Global Database"
menutitle: "Global Database"
weight: 2
chapter: false
pre: "<b></b>"
awsServices: 
    - Amazon Aurora
tags:
    - Warm Standby
---


## Visão geral de bancos de dados globais do Aurora

Um banco de dados global do Aurora consiste em uma região primária da AWS em que os dados são mantidos e em até cinco regiões secundárias da AWS somente leitura. O Aurora replica os dados para a região secundária da AWS com latência típica de menos de um segundo. Você emite operações de gravação diretamente à instância de banco de dados primária na região primária da AWS.

Um banco de dados global do Aurora usa infraestrutura dedicada para replicar seus dados, deixando os recursos do banco de dados disponíveis totalmente para atender às cargas de trabalho do aplicativo. Os aplicativos com uma presença mundial podem usar instâncias do leitor nas regiões secundárias da AWS para leituras de baixa latência. Em alguns casos improváveis, o banco de dados pode ficar degradado ou isolado em uma região da AWS. Se isso acontecer, com um banco de dados global, é possível promover uma das regiões secundárias da AWS para realizar cargas de trabalho de leitura/gravação completas em menos de um minuto.

**Amazon Aurora Global Databases Deep Dive:**
{{< youtube 1vFg1z-2E7Y >}}


**RTO:** 1 minuto.

---
**Referências:**
- [Aurora - Banco de dados Globais](https://docs.aws.amazon.com/pt_br/AmazonRDS/latest/AuroraUserGuide/aurora-global-database.html#aurora-global-database-overview)
- [Aurora - Global Database Lab (MySQL)](https://awsauroralabsmy.com/global/deploy/)
- [AWS Docs - Global Databases](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-global-database.html)
- [Global Databases (PostgreSQL)](https://aws.amazon.com/blogs/database/aurora-postgresql-disaster-recovery-solutions-using-amazon-aurora-global-database/)
- [Write Forwarding (MySQL)](https://aws.amazon.com/blogs/database/building-globally-distributed-mysql-applications-using-write-forwarding-in-amazon-aurora-global-database/)