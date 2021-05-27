---
title: "Amazon CloudFront - Failover com Lambda@Edge"
menutitle: "Failover com Lambda@Edge"
weight: 2
chapter: false
pre: "<b></b>"
awsServices: 
 - Amazon CloudFront
tags: 
 - Active-Active
---

Você pode usar as funções do Lambda@Edge com distribuições do CloudFront que tiver configurado com grupos de origem. Para usar uma função do Lambda, especifique-a em um trigger de solicitação origem ou resposta de origem de um grupo de origem ao criar o comportamento de cache. Ao usar uma função do Lambda@Edge com um grupo de origem, ela poderá ser acionada duas vezes para uma única solicitação de visualizador. Por exemplo, considere este cenário:

1. Você cria uma função do Lambda@Edge com um trigger de solicitação de origem.

2. A função do Lambda é acionada quando o CloudFront envia uma solicitação para a origem primária (em um falha de cache).

3. A origem primária responde com um código de status de HTTP configurado para failover.

4. A função do Lambda é acionada novamente quando o CloudFront envia a mesma solicitação à origem secundária.

![Origins Groups with Lambda@Edge](/images/cloudfront-origingroups-with-lambda-edge.png)

---
**Referências**
- [Uso do failover de origem com funções do Lambda@Edge](https://docs.aws.amazon.com/pt_br/AmazonCloudFront/latest/DeveloperGuide/high_availability_origin_failover.html#concept_origin_groups.lambda)