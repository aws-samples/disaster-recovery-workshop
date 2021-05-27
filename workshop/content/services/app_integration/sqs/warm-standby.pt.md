---
title : "Amazon SQS - Warm Standby"
menuTitle : "Warm Standby"
weight : 5
tags:
 - Warm Standby
awsServices:
 - Amazon SQS
---

Uma estratégia Warm Standby para recursos como filas do Amazon SQS é semelhante ao cenário [Pilot Light]({{< ref "/services/app_integration/sqs/pilot-light" >}} "Pilot Light"). 

Recursos serverless são cobrados conforme o uso, então, enquanto não há tráfego na região de DR, não há custos pelos recursos existentes.

---
**Referências:**
- [Amazon SNS Cross Region delivery to SQS Queues](https://docs.aws.amazon.com/sns/latest/dg/sns-cross-region-delivery.html)