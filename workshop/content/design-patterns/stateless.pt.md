---
title: "Stateless"
menutitle: "Stateless"
weight: 4
chapter: false
pre: "<b>4. </b>"
type: "Documentation"
---



Crie serviços sem estado sempre que possível: serviços não devem exigir estado ou devem descarregar o estado de modo que não haja dependência entre solicitações de clientes diferentes em relação aos dados armazenados localmente no disco ou na memória. Isso permite que os servidores sejam substituídos quando necessário sem causar impacto na disponibilidade. O Amazon ElastiCache ou o Amazon DynamoDB são bons destinos para o estado descarregado.

![Stateless - Amazon Elasticache](/images/stateless.png)


*\*Trecho retirado de [AWS Well-Architected Framework - Reliability](https://docs.aws.amazon.com/pt_br/wellarchitected/latest/reliability-pillar/design-interactions-in-a-distributed-system-to-mitigate-or-withstand-failures.html)*

---
**Referências:**
- [Amazon ElastiCache]({{< ref "elasticache" >}})
- [Stateless Web Tier](https://docs.aws.amazon.com/pt_br/whitepapers/latest/best-practices-wordpress/stateless-web-tier.html)
- [Serverless Today: Stateless Architectures for Stateless Applications](https://www.youtube.com/watch?v=4vwXY0SGnDo)
