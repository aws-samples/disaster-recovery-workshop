---
title: "Disponibilidade"
menutitle: "Disponibilidade"
weight: 1
chapter: false
pre: "<b></b>"
type: Documentation
---

Disponibilidade (também conhecida como disponibilidade de serviços) é uma métrica comumente usada para medir quantitativamente a confiabilidade.

* Disponibilidade é a porcentagem de tempo em que uma carga de trabalho está disponível para uso.

Esse percentual é calculado em períodos como um mês, um ano ou os últimos três anos. Aplicando a interpretação mais estrita possível, a disponibilidade diminui sempre que o aplicativo não está operando normalmente, incluindo interrupções programadas e não programadas. 



{{< youtube Pr7u2dSKS38>}}


Veja a seguir uma tabela das metas de design de disponibilidade de aplicativo comuns e a máxima duração das interrupções que podem ocorrer em um ano sem que o objetivo deixe de ser atingido. A tabela contém exemplos dos tipos de aplicativos comuns em cada nível de disponibilidade.

|Objetivo de Disponibilidadde|Indisponibilidade Máx. (por ano)|RTO|# de Regiões|Tipo de aplicação
|--|--|--|--|--|
|90%|36 dias e 12 horas|4 - 24 horas|Uma|.|
|95%|18 dias e 6 horas|4 - 12 horas|Uma|.|
|[99%](https://docs.aws.amazon.com/pt_br/wellarchitected/latest/reliability-pillar/s-99-scenario.html)|3 dias e 15 horas|2 - 8 horas|Uma|Tarefas de processamento em lote, análise de dados|
|[99,9%](https://docs.aws.amazon.com/pt_br/wellarchitected/latest/reliability-pillar/s-99.9-scenario.html)|8 horas e 45 minutos|1 - 4 horas|Uma|Ferramentas internas de uma empresa|
|[99,95%](https://docs.aws.amazon.com/pt_br/wellarchitected/latest/reliability-pillar/s-99.95-with-a-recovery-time-between-5-and-30-minutes.html)|4 horas e 22 minutos|30 - 120 minutos|Uma ou mais|Comércio online, ponto de vendas|
|[99,99%](https://docs.aws.amazon.com/pt_br/wellarchitected/latest/reliability-pillar/s-99.99-scenario.html)|52 minutos|5 - 20 minutos|Duas ou mais|Entrega de vídeo|
|[99,999%](https://docs.aws.amazon.com/pt_br/wellarchitected/latest/reliability-pillar/s-99.999-or-higher-scenario-with-a-recovery-time-under-1-minute.html)|5 minutos|1 - 60 segundos|Duas ou mais|Transações financeiras|

{{% notice info %}}
Use a tabela acima como uma referência para criar a versão adaptada para a necessidade das suas aplicações. Independente da estratégia de recuperação escolhida, a mesma não pode ultrapassar o o tempo limite de recuperação (RTO).
{{% /notice %}}
<br>

---
**Referências**
- [AWS Well-Architected Framework - Disponibilidade](https://docs.aws.amazon.com/pt_br/wellarchitected/latest/reliability-pillar/availability.html)
- [Failure is not an Option - Designing Highly Resilient AWS Systems](https://www.youtube.com/watch?v=Pr7u2dSKS38)
