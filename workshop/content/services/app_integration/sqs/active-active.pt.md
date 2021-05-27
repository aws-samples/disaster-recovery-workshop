---
title : "Amazon SQS - Active-Active"
menuTitle : "Active-Active"
weight : 5
tags:
  - Active-Active
awsServices: 
  - Amazon SQS
---

Em um cenário de workload multi região ativo-ativo, isto é, diferentes regiões recebem tráfego de clientes dividindo a carga total da solução, a abordagem mais comum é dividir os clientes conforme a localidade para acesso a região mais próxima. Dentro deste cenário, recursos que possuem replicação multi-região podem se basear nesta funcionalidade. Recursos como filas de mensagens não possuem feature de replicação automática das mensagens para outra região e é preciso adotar a melhor política de acordo com o cenário. 
Em relação a infraestrura de suporte das filas, utilizando infraestrutura como código e um pipeline de entrega multi região permite que você provisione os recursos e alterações facilmente. Em relação as mensagens trafegadas, deve-se levar em conta a possibilidade de perda de mensagens e se o sistema de backend está preparado para processar mensagens repetidas (idempotência).
Se nenhuma mensagem puder ser perdida, deve-se garantir a persistência da mensagem em algum mecanismo que tenha replicação global ou deve-se automatizar esta replicação utilizando um padrão Fan-out. 

---
**Referências:**
- [Amazon SNS Cross Region delivery to SQS Queues](https://docs.aws.amazon.com/sns/latest/dg/sns-cross-region-delivery.html)

