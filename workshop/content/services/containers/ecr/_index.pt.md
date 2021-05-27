---
title: "Amazon ECR"
weight: 1
chapter: true
pre: "<b></b>"
awsServices:
 - Amazon EKS
 - Amazon ECR
type: documentation
---

Os containers proporcionam uma maneira padrão de empacotar código, configurações e dependências de seu aplicativo em um único objeto. Eles compartilham um sistema operacional instalado no servidor e são executados como processos isolados. Isso permite fazer implantações rápidas, confiáveis e consistentes, independentemente do ambiente. A plataforma AWS oferece recursos de infraestrutura otimizados para a execução de containers, além de um conjunto de serviços de orquestração que facilitam a criação e execução de aplicativos conteinerizados em produção.

A AWS oferece suporte a containers [compatíveis com OCI](https://opencontainers.org/), inclusive containers de [docker](https://aws.amazon.com/pt/docker/).

---

No *whitepaper [Running Containerized Microservices on AWS](https://d1.awsstatic.com/whitepapers/DevOps/running-containerized-microservices-on-aws.pdf)* são discutidas as práticas recomendadas de arquitetura para adoção de containers na AWS, e como os padrões de design de software tradicionais evoluem no contexto de containers. Os princípios de microsserviços de Martin Fowler são mapeados para [The Twelve-Factor App](https://12factor.net/) pattern, além de considerações da vida real. Depois de ler este conteúdo, você terá um ponto de partida para a construção de microsserviços usando as práticas recomendadas e padrões de projeto de software. 

É recomendado a leitura do *whitepaper* acima, pois o mesmo traz os fundamentos e boas práticas recomendadas de arquitetura para adoção de containers na AWS que são essenciais para uma arquitetura resiliente e com contingência em múltiplas regiões.
