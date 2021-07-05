---
title: "Regiões e Zonas da AWS"
menutitle: "Regiões e Zonas"
weight: 1
chapter: false
pre: "<b></b>"
---


Cada região é projetada para ser isolada das outras regiões. Isso proporciona a maior tolerância a falhas e estabilidade possível.

![Regiões](/images/aws-region2.png)

Já as zonas de disponibilidade são vários *datacenters* isolados em cada região.

{{% notice note %}}
A AWS opera *datacenters* de última geração e altamente disponíveis. Embora sejam raras, podem ocorrer falhas que afetam a disponibilidade das instâncias e serviços que estão no mesmo local. Se você hospedar todas as suas instâncias em um único local afetado por uma falha, nenhuma delas ficará disponível. Por isso é importante considerar o uso de múltiplas zonas de disponibilidade para aplicações em produção, e considerar o uso de múltiplas regiões para aplicações críticas.

{{% /notice %}}

Veja a [Lista de Regiões](https://docs.aws.amazon.com/general/latest/gr/rande.html#region-names-codes) disponíveis na AWS.

---
**Referências:**
- [Regiões e Zonas](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-regions)




