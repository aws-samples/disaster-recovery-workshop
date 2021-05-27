---
title: "Amazon CloudFront - Grupo de Origem"
menutitle: "Grupo de Origem"
weight: 2
chapter: false
pre: "<b></b>"
awsServices: 
 - Amazon CloudFront
tags: 
 - Warm Standby
---

Também é possível configurar o CloudFront failover de origem para cenários que exigem alta disponibilidade. Para começar, crie um grupo de origem com duas origens: uma primária e outra secundária. Se a origem primária estiver indisponível ou retornar códigos de status de resposta HTTP específicos que indiquem falha, o CloudFront alternará automaticamente para a origem secundária.

Para configurar o failover de origem, você deve ter uma distribuição com pelo menos duas origens. Depois, crie um grupo de origens para a distribuição que inclua duas origens, definindo uma delas como a primária. Por último, crie ou atualize um comportamento de cache para usar o grupo de origem.

O CloudFront roteia todas as solicitações recebidas para a origem primária, mesmo quando uma solicitação anterior tenha feito failover para a origem secundária. O CloudFront só envia solicitações para a origem secundária após uma falha de uma solicitação para a origem primária.

O diagrama a seguir mostra como o failover de origem funciona.

![](/images/cloudfront-failover.png)


{{% notice info %}}
O CloudFront faz failover para a origem secundária somente quando o método HTTP da solicitação do consumidor é **GET, HEAD ou OPTIONS**. O CloudFront não faz failover quando o consumidor envia um método HTTP diferente (por exemplo POST, PUT etc.).
{{% /notice %}}

**RTO esperado:** Alguns segundos dependendo da connfiguração de *timeout* para conexão com a origem, número de tentativas e tempo limite de respostas. 

---
**Referências**
- [Controlar tempos limite e tentativas da origem](https://docs.aws.amazon.com/pt_br/AmazonCloudFront/latest/DeveloperGuide/high_availability_origin_failover.html#controlling-attempts-and-timeouts)