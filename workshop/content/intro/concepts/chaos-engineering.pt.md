---
title: "Engenharia do Caos"
menutitle: "Engenharia do Caos"
weight: 2
chapter: false
pre: "<b></b>"
type: documentation
---

Engenharia do Caos é a disciplina de experimentação de uma sistema com o objetivo de aumentar a confiança em sua capacidade de suportar problemas em seu ambiente. 

Testes em ambientes não produtivos, devem ser realizados regularmente, e devem ser parte do ciclo de CI/CD. Em produção, os teams devem realizar os testes de maneira a não provocar a indisponibilidade do serviço. 

Os resultados dos testes devem ser medidos e depois comparados com os objetivos de disponibilidade, para entender se a aplicação executando em um determinando ambiente é capaz de atingir os objetivos definidos.

{{< youtube ztiPjey2rfY >}}

---
### AWS Fault Injection Simulator
O AWS FIS é um serviço dedicado a simular falhas no ambiente AWS, para que seja possível realizar testes de forma controlada. O AWS FIS simplifica o processo de controle de experimentação das aplicações em uma ambiente de falha. Para mais informações, consulte: [AWS Fault Injection Simulator](https://aws.amazon.com/fis/)

---
**Referências:**
- [Princípios de Chaos Engineering](https://principlesofchaos.org/pt)
