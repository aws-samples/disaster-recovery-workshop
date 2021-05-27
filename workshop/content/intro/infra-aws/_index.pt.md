---
title: "Infraestrutura Global da AWS"
menuTitle: "Infraestrutura Global"
weight: 30
chapter: false
pre: "<b>3. </b>"
---

Para o trabalho de planejamento de estratégias de recuperação de desastres é importante conhecer a infraestrutura global da AWS. É necessário o uso de ao menos 2 regiões para evitar impactos de disrupção de serviços em nível de região. Veja o mapa da infraestrutura AWS e escolha as regiões que serão usadas por suas aplicações.
Acesse [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/).


{{% notice tip %}}
<i class="fas fa-lightbulb"></i>
As regiões são geograficamente distribuídas e isso reflete no tempo de comunicação entre elas. Se sua aplicação é sensível a latência, você deve equilibrar a proximidade das regiões com a possibilidade de ocorrência de um evento regional.
{{% /notice %}}


Existem serviços que não estão disponíveis em todas as regiões, ao escolher uma região, confirme se o serviço que deseja utilizar está disponível. Veja a tabela de [Serviços Regionais da AWS](https://aws.amazon.com/pt/about-aws/global-infrastructure/regional-product-services/).

---
**Referências:**
- [AWS Global Cloud Infrastructure](hhttps://aws.amazon.com/about-aws/global-infrastructure/)