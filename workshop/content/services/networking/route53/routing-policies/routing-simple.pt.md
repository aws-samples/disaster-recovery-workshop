---
title: "Amazon Route 53 - Política de Roteamento Simples"
menutitle: "Simples"
weight: 3
chapter: false
pre: "<b></b>"
awsServices:
  - "Amazon Route 53"
tags: 
 - Active-Active
type: Documentation
---

O roteamento simples permite configurar registros de DNS padrão sem roteamento especial do Route 53, como ponderados ou de latência. Com o roteamento simples, você normalmente roteia o tráfego para um único recurso, por exemplo, para um servidor web do seu site.

Se você escolher a política de roteamento simples no console do Route 53, não poderá criar vários registros com o mesmo nome e tipo, mas poderá especificar diversos valores no mesmo registro, como vários endereços IP. (Se escolher a política de encaminhamento simples para um registo alternativo, só pode especificar um recurso AWS ou um registo na zona alojada actual.) Se especificar vários valores num registo, Route 53 apresenta todos os valores para o resolver recursivo por ordem aleatória e o resolver apresenta os valores para o cliente (tal como um browser da Web) que submeteu a consulta DNS. Em seguida, o cliente escolhe um valor e reenvia a consulta.

---
**Referências**
- [Amazon Route53 - Roteamento Simples](https://docs.aws.amazon.com/pt_br/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-simple)