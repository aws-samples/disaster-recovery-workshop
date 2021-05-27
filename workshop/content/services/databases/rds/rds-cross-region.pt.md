---
title: "RDS - Replicação entre Regiões"
menutitle: "Replicação entre Regiões"
weight: 2
chapter: false
pre: "<b></b>"
awsServices: 
 - "Amazon RDS"
tags:
 - Warm Standby
---

O Amazon RDS usa a funcionalidade de replicação integrada nos mecanismos de banco de dados MariaDB, MySQL, Oracle, PostgreSQL e Microsoft SQL Server para criar um tipo especial de instância de banco de dados chamada de réplica de leitura a partir de uma instância de banco de dados de origem. 

Uma réplica de leitura pode estar em uma região da AWS diferente da região da instância de banco de dados primária. Nesses casos, o Amazon RDS configura um canal de comunicação seguro entre a instância de banco de dados primária e a réplica de leitura. O Amazon RDS estabelece as configurações de segurança da AWS necessárias para permitir o canal seguro, como a inclusão de entradas de grupos de segurança.

Você só pode replicar entre Regiões de AWS ao usar as seguintes instâncias de banco de dados do Amazon RDS:

- MariaDB (todas as versões).
- MySQL versão 5.6 e posterior.
- Oracle Enterprise Edition (EE), versão de mecanismo 12.1.0.2.v10 e versões 12.1 superiores, todas as versões de 12.2 e todas as versões de 18.0. É necessária uma licença do Active Data Guard. Para obter informações sobre limitações para réplicas de leitura entre regiões do Oracle, consulte Requisitos de réplica para o Oracle.
- PostgreSQL (todas as versões).

---
**Referências:**

- [Como trabalhar com réplicas de leitura](https://docs.aws.amazon.com/pt_br/AmazonRDS/latest/UserGuide/USER_ReadRepl.html)
- [Cross-Region disaster recovery of Amazon RDS for SQL Server](https://aws.amazon.com/blogs/database/cross-region-disaster-recovery-of-amazon-rds-for-sql-server/)
- [Best practices for Amazon RDS for PostgreSQL cross-Region read replicas](https://aws.amazon.com/pt/blogs/database/best-practices-for-amazon-rds-for-postgresql-cross-region-read-replicas/)
- [Implementing a disaster recovery strategy with Amazon RDS](https://aws.amazon.com/blogs/database/implementing-a-disaster-recovery-strategy-with-amazon-rds/)
- [Implementing cross-region disaster recovery using Oracle GoldenGate for Amazon RDS for Oracle](https://aws.amazon.com/blogs/database/implementing-cross-region-disaster-recovery-using-oracle-goldengate-for-amazon-rds-for-oracle/)
- [Criar uma réplica de leitura em uma região da AWS diferente](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReadRepl.html?ref=wellarchitected#USER_ReadRepl.XRgn)
