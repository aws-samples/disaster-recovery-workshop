---
title: "Amazon Route53 - Política de Roteamento Failover"
menutitle: "Failover"
weight: 3
chapter: true
pre: "<b></b>"
awsServices:
 - "Amazon Route53"
tags: 
 - Pilot Light
type: Documentation
---

O roteamento de failover permite rotear o tráfego para um recurso quando o recurso estiver íntegro ou para um recurso diferente quando o primeiro recurso não estiver íntegro. Os registros primários e secundários podem rotear o tráfego para qualquer ponto a partir de um bucket do Amazon S3 que é configurado como um site para uma árvore complexa de registros. Para obter mais informações, consulte [Failover ativo/passivo](https://docs.aws.amazon.com/pt_br/Route53/latest/DeveloperGuide/dns-failover-types.html#dns-failover-types-active-passive).