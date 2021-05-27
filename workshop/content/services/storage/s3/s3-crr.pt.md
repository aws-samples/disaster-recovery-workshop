---
title: "Amazon S3 - Replicação entre Regiões"
menutitle: "Replicação entre Regiões"
weight: 2
chapter: false
pre: "<b></b>"
awsServices: 
    - Amazon S3
tags:
    - Active-Active
type: documentation
---


## Mecanismo de Replicação
A [replicação](https://docs.aws.amazon.com/pt_br/AmazonS3/latest/dev/replication.html) permite a cópia assíncrona automática de objetos em buckets do Amazon S3. Os buckets configurados para replicação de objetos podem ser de propriedade da mesma conta da AWS ou de contas diferentes. Você pode copiar objetos entre **regiões diferentes da AWS** ou dentro da mesma região.

**Replicar objetos em 15 minutos:** você pode usar o Controle do tempo de replicação do S3 (S3 RTC) para replicar seus dados na mesma região da AWS ou em diferentes regiões em um período previsível. O S3 RTC replica 99,99% dos novos objetos armazenados no Amazon S3 em 15 minutos (respaldado por um acordo de nível de serviço).

---
**Referências:**
- [LAB: S3 - Replicação Bidirecional]({{< ref "bidire-rep-s3" >}})
- [Atendimento aos requisitos de conformidade usando o Controle do tempo de replicação do S3 (S3 RTC)](https://docs.aws.amazon.com/pt_br/AmazonS3/latest/userguide/replication-time-control.html)
