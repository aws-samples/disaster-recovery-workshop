---
title: "Aurora - Backup de Snapshots"
menutitle: "Backup de Snapshots"
chapter: false
pre: "<b></b>"
awsServices: 
    - Amazon Aurora
tags:
    - Backup & Restore
---

O Aurora faz backup do volume de cluster automaticamente e mantém dados de restauração pelo período de retenção de backup. Os backups do Aurora são contínuos e incrementais para que você possa restaurar rapidamente de qualquer ponto dentro do período de retenção de backup. Quando os dados do backup estão sendo gravados, não há nenhum impacto ao desempenho ou interrupção de serviço do banco de dados. Você pode especificar um período de retenção de backup, de 1 a 35 dias, ao criar ou modificar um cluster de banco de dados. Os backups do Aurora são armazenados no Amazon S3. 

Com o Amazon RDS você pode copiar snapshots manuais ou automatizados do cluster de banco de dados. Depois de copiar um snapshot, a cópia é um snapshot manual.
Você pode copiar um snapshot dentro da mesma região da AWS, um snapshot entre regiões da AWS e snapshots compartilhados.

Você não pode copiar um snapshot de cluster de banco de dados entre regiões e contas em uma única etapa. Realize uma etapa para cada uma dessas ações de cópia. Como alternativa à cópia, você também pode compartilhar snapshots manuais com outras contas da AWS. Para obter mais informações, consulte [Compartilhar um snapshot do cluster de banco de dados](https://docs.aws.amazon.com/pt_br/AmazonRDS/latest/AuroraUserGuide/USER_ShareSnapshot.html). 

É possível utilizar o AWS Backup para gerenciar os backups do cluster do Amazon Aurora. O AWS Backup pode configurar centralmente as políticas de backup, monitorar a atividade de backup, copiar um snapshot dentro e entre as regiões da AWS.

---
**Referências:**
- [Aurora - Copiar um snapshot de cluster de banco de dados](https://docs.aws.amazon.com/pt_br/AmazonRDS/latest/AuroraUserGuide/USER_CopySnapshot.html)
- [Noções básicas do uso do armazenamento de backup do Aurora](https://docs.aws.amazon.com/pt_br/AmazonRDS/latest/AuroraUserGuide/aurora-storage-backup.html)
- [AWS Backup - Aurora](https://aws.amazon.com/about-aws/whats-new/2020/06/amazon-aurora-snapshots-can-be-managed-via-aws-backup/?nc1=h_ls)