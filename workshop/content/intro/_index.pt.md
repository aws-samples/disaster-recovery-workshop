---
title: "Introdução"
weight: 10
chapter: true
pre: "<b>1. </b>"
---

Esse material discute fundamentos de arquitetura de software e mecanismos para construção de aplicações altamente resilientes no ambiente Amazon Web Services (AWS). O objeto é atender os requisitos de recuperação de aplicações em diferentes camadas, inclusive para aplicações de missão crítica.

Aplicações resilientes provêm serviço contínuo apesar das disrupções. 
Eventos como desastres naturais, falhas de hardware e erros humanos podem causar interrupções em um serviço. Empresas que não preparam suas aplicações para essas falhas possuem um maior risco de *downtime*, instabilidade e perda de dados em suas soluções. Essa indisponibilidade pode acarretar em perda financeira, implicações legais, impactos negativos sobre a marca e a insatisfação dos clientes.<sup>[1](https://d1.awsstatic.com/Industries/Financial%20Services/Overview/Resilient%20Applications%20on%20AWS%20for%20Financial%20Services.pdf)</sup>

Desenvolver uma aplicação considerando as possíveis falhas, é um dos príncipios mais importantes para que ela seja resiliente. 
De acordo com a criticidade de cada aplicação, devem ser estipulados RTO e RPO esperados.
É importante que sejam discutidas as [Estratégias de Recuperação de Desastres](disaster-recovery) no ambiente AWS.

Para entender como funciona a infraestrutura da AWS, veja [Infraestrutura Global](global-infra).
Caso seja o seu primeiro contato com o ambiente AWS veja também [Regiões e Zonas de Disponibilidade](regions-az). 


---
**Referências:**
1. [Building Mission-Critical Financial Services Applications on AWS](https://d1.awsstatic.com/Industries/Financial%20Services/Overview/Resilient%20Applications%20on%20AWS%20for%20Financial%20Services.pdf)
