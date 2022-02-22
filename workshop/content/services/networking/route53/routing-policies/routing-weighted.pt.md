---
title: "Amazon Route 53 - Política de Roteamento Ponderado"
menutitle: "Ponderado (Weighted)"
weight: 3
chapter: false
pre: "<b></b>"
awsServices:
 - "Amazon Route 53"
tags: 
 - Warm Standby
type: Documentation
---

O roteamento ponderado permite que você associe vários recursos a um único nome de domínio (exemplo.com) ou subdomínio (acme.exemplo.com) e escolha a quantidade de tráfego roteado para cada recurso. Isso pode ser útil para diversas finalidades, incluindo balanceamento de carga e teste de novas versões de software.

Para configurar o roteamento ponderado, crie registros que têm o mesmo nome e tipo de cada um dos seus recursos. A cada registro você atribui um peso relativo que corresponde à quantidade de tráfego que deseja enviar a cada recurso. O Amazon Route 53 envia o tráfego a um recurso com base no peso que você atribui ao registro como uma proporção do peso total para todos os registros no grupo:

![Roteamento Ponderado](/images/route53-weighted.png)

O roteamento ponderado permite que você associe vários recursos a um único nome de domínio (exemplo.com) ou subdomínio (acme.exemplo.com) e escolha a quantidade de tráfego roteado para cada recurso. Isso pode ser útil para diversas finalidades, incluindo balanceamento de carga e teste de novas versões de software.

Para configurar o roteamento ponderado, crie registros que têm o mesmo nome e tipo de cada um dos seus recursos. A cada registro você atribui um peso relativo que corresponde à quantidade de tráfego que deseja enviar a cada recurso. O Amazon Route 53 envia o tráfego a um recurso com base no peso que você atribui ao registro como uma proporção do peso total para todos os registros no grupo: