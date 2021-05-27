---
title : "Amazon SQS - Pilot Light"
menuTitle : "Pilot Light"
weight : 5
tags:
 - Pilot Light
awsServices:
 - Amazon SQS
---

Recursos serverless como as filas do SQS podem suportar facilmente uma arquitetura Pilot Light através do uso de um pipeline de entrega de infraestrutura multi região usando, por exemplo, o framework SAM (Serverless Application Model). Todo novo recurso ou alteração em recurso existente passa por um pipeline de integração e entrega para validação e deploy em mais de uma região. Uma vez que seja necessária a ativação da região nova, basta chavear as requisições para a nova região fazendo uso dos recursos já existentes e que não oferecem custo quando não já demanda.

---
**Referências:**
- [Amazon SNS Cross Region delivery to SQS Queues](https://docs.aws.amazon.com/sns/latest/dg/sns-cross-region-delivery.html)
- [AWS Serverless Application Model](https://aws.amazon.com/serverless/sam/)