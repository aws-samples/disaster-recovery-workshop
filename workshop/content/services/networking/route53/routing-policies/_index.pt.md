---
title: "Amazon Route53 - Políticas de Roteamento"
menutitle: "Políticas de Roteamento"
weight: 3
chapter: true
pre: "<b></b>"
awsServices:
 - "Amazon Route53"
type: Documentation
---

Quando você cria um registro, é possível escolher uma política de roteamento, o que determina como o Amazon Route 53 responde a consultas:

**Política de roteamento simples** – use para um único recurso que executa uma determinada função para seu domínio, por exemplo, um servidor web que oferece conteúdo para o site exemplo.com.

**Política de roteamento de failover** – use quando quiser configurar o failover ativo-passivo.

**Política de roteamento de localização geográfica** – use quando quiser rotear o tráfego com base na localização dos usuários.

**Política de roteamento de proximidade geográfica** – use quando desejar rotear o tráfego com base no local de seus recursos e, opcionalmente, alternar o tráfego de recursos em um local para recursos em outro local.

**Política de roteamento de latência** – use quando você tiver recursos em várias regiões da AWS e quiser rotear o tráfego para a região que fornece a melhor latência.

**Política de roteamento de resposta com múltiplos valores** – use quando quiser que o Route 53 responda a consultas DNS com até oito registros íntegros selecionados aleatoriamente.

**Política de roteamento ponderado** – use para rotear o tráfego para vários recursos nas proporções que você especificar.

{{% notice tip %}}
<i class="fas fa-lightbulb"></i>
A escolha da política de roteamento deve seguir a estratégia de recuperação de desastres que está sendo implementada. Veja a tabela de sugestão abaixo:
{{% /notice %}}

| Política  | Estratégia  |
|---|---|
| [Simples (Múltiplos IP's)](https://docs.aws.amazon.com/pt_br/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-simple) <br> [Ponderado (50%-50%)](https://docs.aws.amazon.com/pt_br/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-weighted) <br> [Múltiplos Valores](https://docs.aws.amazon.com/pt_br/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-multivalue) | Active-Active | 
| [Ponderado (Weighted)](https://docs.aws.amazon.com/pt_br/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-weighted) | Warm Standby | 
| [Failover](https://docs.aws.amazon.com/pt_br/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-failover) | Pilot Light| 

<br>

{{< youtube WDDkLOT8SCk>}}

---
**Referências:**
- [Amazon Route53 - Política de Roteamento](https://docs.aws.amazon.com/pt_br/Route53/latest/DeveloperGuide/routing-policy.html)
- [Amazon Builders'Library - Isolamento de carga de trabalho usando ordem aleatória de fragmentação](https://aws.amazon.com/pt/builders-library/workload-isolation-using-shuffle-sharding/)