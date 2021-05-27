---
title: "Amazon Route53 - Health Checks"
menutitle: "Health Checks"
weight: 3
chapter: true
pre: "<b></b>"
awsServices:
 - "Amazon Route53"
type: documenntation
---

Crie um *health check*, se quiser que o Amazon Route 53 verifique o estado de saúde de um endereço que será usado para responder as consultas de DNS utilizando este registo. Essa verificação funciona como um filtro, portanto serão retornados apenas endereços que passaram na avaliação, que estão como saudáveis.

![Route53 - Health Check](/images/route53-health-checks.png)

{{% notice warning %}}
Se você criar um *health check* para o próprio registro de DNS e, em seguida, associar esse registro de DNS ao mesmo *health check*, os resultados do *health check* serão imprevisíveis.
{{% /notice %}}

{{< youtube vGywoYc_sA8>}}

---
**Referências**
- [Amazon Route53 - Health Checks (Verificação de Integridade)](https://docs.aws.amazon.com/pt_br/Route53/latest/DeveloperGuide/dns-failover-determining-health-of-endpoints.html)