---
title : "Amazon SQS - Backup & Restore"
menuTitle : "Backup & Restore"
weight : 5
tags:
  - Backup & Restore
awsServices:
  - Amazon SQS
---

De forma geral, tratando-se do conceito de filas de mensagens onde temos produtores de mensagens e consumidores, no mundo ideal, a profundidade de uma fila deve sempre tender a zero mensagens dado que os consumidores consomem imediatamente todas as mensagens geradas pelos produtores. Contudo, existem outras razões que podem levar ao acúmulo de mensagens como:
* Necessidade de controle de fluxo de mensagens para evitar sobrecarga de processamento no backend
* Uso de filas para armazenamento temporário para processamento batch agendado
* Fila DLQ que armanezam mensagens que não puderam ser processadas por erro no consumidor ou cenário de poison pill

Pensando nestes casos, é importante pensar em alternativas para fazer o armazenamento destas mensagens em um cenário de indisponibilidade do serviço em uma região. 
Uma abordagem para permitir o compartilhamento das mensagens com outras regiões de forma fácil é utilizando o [Extended Client do SQS](https://github.com/awslabs/amazon-sqs-java-extended-client-lib). Com ele, é possível enviar todas as mensagens para o S3 utilizando uma configuração simples. Este recurso é bastante útil para lidar com mensagens de tamanho superior ao [máximo suportado pelo Amazon SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/quotas-messages.html).