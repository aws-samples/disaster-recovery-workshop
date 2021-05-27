---
title: "Amazon DynamoDB"
menutitle: "Amazon DynamoDB"
weight: 2
chapter: true
pre: "<b></b>"
awsServices: Amazon DynamoDB
---

O Amazon DynamoDB é um serviço de banco de dados NoSQL totalmente gerenciado que fornece desempenho rápido e previsível com escalabilidade contínua. O DynamoDB permite que você transfira as cargas administrativas de operação e escalabilidade de um banco de dados distribuído, de forma que não precise se preocupar com provisionamento, instalação e configuração de hardware, replicação e correção de software nem com escalabilidade de cluster. O DynamoDB também oferece criptografia em repouso, que elimina a carga operacional e a complexidade envolvida na proteção de dados confidenciais.

Com o DynamoDB, é possível criar tabelas de banco de dados que armazenam e recuperam qualquer quantidade de dados e atendem a todos os níveis de tráfego solicitados. Também é possível aumentar ou reduzir a capacidade de taxa de transferência das tabelas sem tempo de inatividade ou degradação do desempenho. É possível usar o Console de gerenciamento da AWS para monitorar a utilização de recursos e métricas de desempenho.

O DynamoDB oferece o recurso de backup sob demanda. Ele permite que você crie backups completos das suas tabelas para retenção e arquivamento de longo prazo de modo a atender às necessidades de conformidade regulamentar. Para obter mais informações, consulte [Backup e restauração sob demanda para o DynamoDB](https://docs.aws.amazon.com/pt_br/amazondynamodb/latest/developerguide/BackupRestore.html).

É possível criar backups sob demanda, bem como habilitar a recuperação point-in-time para as tabelas do Amazon DynamoDB. A recuperação point-in-time ajuda a proteger as tabelas contra operações acidentais de gravação ou exclusão. Com a recuperação point-in-time, você pode recuperar a tabela para qualquer ponto durante os últimos 35 dias. Para obter mais informações, consulte [Recuperação point-in-time: Como ela funciona](https://docs.aws.amazon.com/pt_br/amazondynamodb/latest/developerguide/PointInTimeRecovery_Howitworks.html).

### Alta disponibilidade e durabilidade
O DynamoDB distribui automaticamente os dados e o tráfego para as tabelas, entre um número suficiente de servidores, para lidar com seus requisitos de throughput e armazenamento, sem deixar de manter um desempenho consistente e rápido. Todos os dados são armazenados em discos de estado sólido (SSDs) e são automaticamente replicados entre várias zonas de disponibilidade em uma região da AWS, o que oferece alta durabilidade de dados e disponibilidade integradas. É possível usar tabelas globais para manter as tabelas do DynamoDB sincronizadas nas regiões da AWS.


---
**Referências:**
- [Amazon DynamoDB](https://docs.aws.amazon.com/pt_br/amazondynamodb/latest/developerguide/Introduction.html)
- [AWS Developer Workshop: How to Build Multi-Region Applications in the Cloud](https://www.youtube.com/watch?v=k9_9bDZa_EI)
- [AWS Solutions: Multi-Region Availability with Amazon DynamoDB, Amazon S3, and Amazon Cognito (LIVE)](https://www.youtube.com/watch?v=tTQ36qQF_vA)
- [AWS re:Invent 2018: How Oath Built a Multi-Region GDPR Application with Amazon DynamoDB (DAT325)](https://www.youtube.com/watch?v=HCfoKyoimOg)