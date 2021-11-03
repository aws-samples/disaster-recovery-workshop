---
title: "Amazon EC2 - Recuperação de Falhas usando CloudEndure"
menutitle: "AWS CloudEndure Disaster Recovery"
weight: 2
chapter: true
pre: "<b></b>"
awsServices: 
 - Amazon EC2
tags:
 - Pilot Light
---

## Continuidade de negócios dimensionável e econômica para servidores físicos, virtuais e de nuvem

Os desastres na área de TI, como falhas de datacenter, corrupção de servidores ou ataques cibernéticos, podem não apenas interromper seus negócios, mas também causar perda de dados, afetar receitas e prejudicar a reputação. O CloudEndure Disaster Recovery minimiza o tempo de inatividade e a perda de dados, fornecendo uma recuperação rápida e confiável de servidores físicos, virtuais e baseados em nuvem na AWS.
Você pode usar o CloudEndure Disaster Recovery para proteger seus bancos de dados mais essenciais, incluindo Oracle, MySQL e SQL Server, além de aplicativos corporativos, como o SAP.
O CloudEndure Disaster Recovery replica continuamente suas máquinas (incluindo o sistema operacional, uma configuração do status do sistema, bancos de dados, aplicativos e arquivos) em uma área de preparação de baixo custo na sua conta AWS e uma região de preferência. No caso de um desastre, você pode instruir o CloudEndure Disaster Recovery a iniciar automaticamente milhares de suas máquinas em seu estado totalmente provisionado em minutos.
Ao replicar suas máquinas em uma área de armazenamento temporário de baixo custo e ao mesmo tempo poder lançar máquinas totalmente provisionadas em minutos, o CloudEndure Disaster Recovery pode reduzir significativamente o custo da infraestrutura de recuperação de desastres.


## Como funciona?

![CloudEndure Disaster Recovery](/images/ec2-cloudendure-dr.png)


## Replicação contínua de dados

O CloudEndure Disaster Recovery fornece replicação contínua, assíncrona e em nível de bloco das suas máquinas origem em uma área de escala. Isso permite que você alcance objetivos de ponto de recuperação (RPOs) subsecundários, como aplicativos atualizados sempre estão prontos para serem adaptados na AWS em caso de desastre. 


## Área de escala de baixo custo

Dados são continuamente mantidos em sincronização e uma área de escala leve na sua região da AWS alvo. A área de escala contém recursos de baixo custo que são automaticamente fornecidos e gerenciados pelo CloudEndure Disaster Recovery. Isso elimina a necessidade de recursos duplicados e reduz de maneira significativa seu custo total de propriedade (TCO – total cost of ownership) da recuperação de desastre.


## Conversão e orquestração da máquina automatizada

No caso de um desastre ou teste, o CloudEndure Disaster Recovery aciona um processo de conversão de máquina altamente automatizado e um mecanismo de orquestração escalável que processa rapidamente milhares de máquinas na região de destino da AWS em paralelo. Isso permite a realização de objetivos de tempo de recuperação (RTOs) em minutos. Diferente de outras soluções de nível de aplicativo, o CloudEndure Disaster Recovery replica máquinas inteiras, inclusive OS, configuração de estado do sistema, discos do sistema, bancos de dados, aplicativos e arquivos. Portanto, quando você iniciar suas máquinas durante uma falha ou teste, suas máquinas funcionarão exatamente como o fazem na sua infraestrutura fonte. Você não precisa instalar tudo de novo ou manter cópias duplicadas do OS, configuração do estado do sistema ou software.


## Recuperação point-in-time

A recuperação granular point-in-time permite recuperar aplicativos e ambientes de TI que foram corrompidos como resultado de alterações acidentais do sistema, ransomware ou outros ataques maliciosos. Nesses casos, você pode iniciar aplicativos de um ponto anterior consistente, em vez de iniciar aplicativos no seu estado mais atualizado. Durante a recuperação, você pode selecionar o último estado ou um estado anterior de uma lista de pontos no tempo.


## Testes fáceis e sem interrupções

Com o CloudEndure Disaster Recovery, você pode conduzir testes de recuperação de desastre sem atrapalhar seu ambiente fonte ou sem o risco de perder dados. Durante os testes, o CloudEndure Disaster Recovery processa máquinas na região AWS de destino em completo isolamento para evitar conflitos de rede e impacto no desempenho. Como você está aproveitando a AWS, não há necessidade de provisionar ou pagar antecipadamente pelos recursos necessários durante a pesquisa de recuperação de desastres. A automação do CloudEndure Disaster Recovery também minimiza o trabalho manual necessário para a realização de testes.


## Suporte abrangente para aplicativos e infraestrutura

Como o CloudEndure Disaster Recovery replica dados no nível de bloco, você pode usá-lo para todos os aplicativos e bancos de dados que funcionam em versões suportadas do Windows e Linux OS. Isso inclui o Windows Server versões 2003/2008/2012/2016/2019 e distribuições do Linux, como CentOS, RHEL, OEL, SUSE, Ubuntu e Debian. 

Para mais informações veja o artigo: [AWS CloudEndure - How CloudEndure Disaster Recovery Works](https://d1.awsstatic.com/products/CloudEndure/How%20_CloudEndure_Disaster_Recovery_Works.pdf)





---
**Referências**
- [Increasing application resilience with CloudEndure Disaster Recovery](https://aws.amazon.com/blogs/storage/increasing-application-resilience-with-cloudendure-disaster-recovery/)
- [Automatizando sua replicação para Disaster Recovery com CloudEndure e Ansible](https://aws.amazon.com/pt/blogs/aws-brasil/automatizando-sua-replicacao-para-disaster-recovery-com-cloud-endure-e-ansible/
)







