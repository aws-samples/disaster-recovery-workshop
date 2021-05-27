---
title: "Amazon DynamoDB - Tabelas Globais"
menutitle: "Global Tables"
weight: 1
chapter: false
pre: "<b></b>"
awsServices:
 - Amazon DynamoDB
tags: 
 - Active-Active
---

## Tabelas Globais
Uma tabela global é a coleção de uma ou mais tabelas-réplica, todas pertencentes a uma única conta da AWS.

Uma tabela-réplica (ou apenas réplica) é uma única tabela do DynamoDB que funciona como parte de uma tabela global. Cada réplica armazena o mesmo conjunto de itens de dados. Qualquer determinada tabela global só pode ter uma tabela-réplica por região da AWS.

Ao criar uma tabela global do DynamoDB, ela consiste em várias tabelas-réplica (uma por região) que o DynamoDB trata como uma única unidade. Cada réplica possui o mesmo nome de tabela e o mesmo esquema de chave primária. Quando um aplicativo grava dados em uma tabela-réplica em uma região, o DynamoDB propaga a gravação para as outras tabelas-réplica nas outras regiões da AWS automaticamente.

{{< youtube k9_9bDZa_EI>}}

## Consistência e resolução de conflitos
Um aplicativo pode ler e gravar dados em qualquer tabela-réplica. Se seu aplicativo usar somente leituras eventualmente consistentes e somente leituras de problemas em uma região da AWS, ele funcionará sem qualquer modificação. No entanto, se o aplicativo exigir leituras fortemente consistentes, ele deverá executar todas as suas leituras e gravações fortemente consistentes na mesma região. O DynamoDB não oferece suporte a leituras fortemente consistentes entre regiões. Portanto, se você gravar em uma região e ler em outra, a resposta lida poderá incluir dados obsoletos que não refletem os resultados de gravações recém-concluídas na outra região.

*Trecho retirado [Amazon DynamoDB Documentation](https://docs.aws.amazon.com/pt_br/amazondynamodb/latest/developerguide/V2globaltables_HowItWorks.html)*

---
**Referênncias:**
- [AWS Developer Workshop: How to Build Multi-Region Applications in the Cloud](https://www.youtube.com/watch?v=k9_9bDZa_EI)
- [AWS Solutions: Multi-Region Availability with Amazon DynamoDB, Amazon S3, and Amazon Cognito (LIVE)](https://www.youtube.com/watch?v=tTQ36qQF_vA)
- [AWS re:Invent 2018: How Oath Built a Multi-Region GDPR Application with Amazon DynamoDB (DAT325)](https://www.youtube.com/watch?v=HCfoKyoimOg)