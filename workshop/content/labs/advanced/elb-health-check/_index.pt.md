---
title: "Amazon ELB - Health Checks "
menutitle: "ELB - Health Checks"
chapter: false
weight: 2
tags:
  - Active-Active
type: Lab
draft: true
---

### Objetivo

Esse exercício irá mostrar os passos para o uso de *Health Checks* usando o Amazon Application Load Balancer (ALB).

**Ao final desse exercício, você será capaz de:**
- Implementar um processo de degradação para transformar dependências fortes em dependências fracas
- Monitorar sua aplicação para detectar falhas
- Rotear o tráfego apenas para instâncias funcionais
- Configurar os comportamentos de *fail-open* e  *fail-closed* em resposta as falhas detectadas
- Usar serviços AWS para reduzir o tempo médito de recuperaçcão (MTTR)

### Visão Geral da Solução
![ELB - Health Checks](/images/alb-health-check.png)


### Execução
{{< tabs name="labs_types" >}} 
{{< tab name="AWS Console" include="console" />}}  
{{< /tabs >}}

---

#### Autor

* Seth Eliot, Principal Reliability Solutions Architect, AWS Well-Architected


**Referências:**
- [AWS Well-Architected Labs: Implementing Health Checks and Managing Dependencies to Improve Reliability](https://wellarchitectedlabs.com/reliability/300_labs/300_health_checks_and_dependencies/)
- [Amazon Builders' Library - Como implementar verificações de integridade](https://aws.amazon.com/pt/builders-library/implementing-health-checks/)

