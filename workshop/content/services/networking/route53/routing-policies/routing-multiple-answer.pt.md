---
title: "Amazon Route53 - Política de Roteamento de Resposta Múltipla"
menutitle: "Resposta Múltipla"
weight: 4
chapter: false
pre: "<b></b>"
awsServices:
  - "Amazon Route53"
tags: 
 - Active-Active
type: Documentation
---

O roteamento de resposta com valores múltiplos permite que você configure o Amazon Route 53 para retornar vários valores, como endereços IP de seus servidores web, em resposta às consultas de DNS. Você pode especificar vários valores para praticamente qualquer registro, mas o roteamento de resposta com valores múltiplos também permite que você verifique a integridade de cada recurso de modo que o Route 53 retorna somente valores para recursos íntegros. Este tipo de roteamento não substitui o load balancer. No entanto, a capacidade de retornar vários endereços IP cuja integridade pode ser verificada é uma forma de usar o DNS para aprimorar a disponibilidade e o balanceamento de carga.

Para rotear o tráfego de forma aproximada e aleatória para vários recursos, como servidores web, você cria um registro de resposta de vários valores para cada recurso e, opcionalmente, associa uma verificação de integridade do Route 53 a cada registro. O Route 53 responde às consultas DNS com até oito registros íntegros e fornece respostas diferentes para diferentes resolvedores DNS. Se um servidor web se tornar indisponível depois que um resolvedor armazenar uma resposta em cache, o software cliente pode tentar outro endereço IP na resposta.

Observe o seguinte:

- Se você associar uma verificação de integridade ao registro de resposta de múltiplos valores, o Route 53 responderá às consultas de DNS com o endereço IP correspondente apenas quando a verificação de integridade for íntegra.

- Se você não associar uma verificação de integridade a um registro de resposta de múltiplos valores, o Route 53 sempre considerará o registro íntegro.

- Se você tem oito ou menos registros íntegros, o Route 53 responde a todas as consultas de DNS com todos os registros íntegros.

- Quando nenhum dos registros for íntegro, o Route 53 responderá às consultas de DNS com até oito registros com problemas de integridade.

---
**Referências**
- [Roteamento de resposta com vários valores](https://docs.aws.amazon.com/pt_br/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-multivalue)