---
title: "RDS - Backups entre regiões"
menutitle: "Backups entre regiões"
weight: 2
chapter: false
pre: "<b></b>"
awsServices: 
 - "Amazon RDS"
tags:
 - "Backup & Restore"
---

O Amazon RDS cria e salva backups automatizados de sua instância de banco de dados durante a janela de backup de sua instância de banco de dados. O RDS cria um snapshot do volume de armazenamento da instância de banco de dados, fazendo o backup de toda a instância de banco de dados e não apenas dos bancos de dados individuais. O RDS salva os backups automatizados da instância de banco de dados de acordo com o período de retenção de backup especificado. Se necessário, você poderá recuperar seu banco de dados em qualquer ponto no tempo durante o período de retenção do backup

O armazenamento de backup do Amazon RDS para cada região é composto de backups automáticos e snapshots de banco de dados manuais para essa região. O espaço total do armazenamento de backup é igual à soma do armazenamento de todos os backups nessa região. Mover um snapshot de banco de dados para outra região aumenta o armazenamento de backup na região de destino. Os backups são armazenados no Amazon S3. Você pode copiar um snapshot dentro da mesma região da AWS, um snapshot entre regiões da AWS e snapshots compartilhados.

Usando o AWS Backup, é possível copiar backups em várias regiões da AWS sob demanda ou automaticamente como parte de um plano de backup programado. A replicação entre regiões é particularmente valiosa se você tiver requisitos de continuidade dos negócios ou de conformidade para armazenar backups a uma distância mínima dos dados de produção. 

{{< youtube 7gNXfo5HZN8 >}}


---
**Referências:**

- [Como copiar um snapshot entre regiões](https://docs.aws.amazon.com/pt_br/AmazonRDS/latest/UserGuide/USER_CopySnapshot.html)
- [AWS Backup - Backups entre regiões](https://docs.aws.amazon.com/pt_br/aws-backup/latest/devguide/how-it-works-cross-region-replication.html)
- [Replicating automated backups to another AWS Region](https://docs.aws.amazon.com/pt_br/AmazonRDS/latest/UserGuide/USER_ReplicateBackups.html)
- [Automate RDS Snapshots for disaster recovery](https://github.com/aws-samples/ssm-for-rds-snapshot-cross-region-copy)





