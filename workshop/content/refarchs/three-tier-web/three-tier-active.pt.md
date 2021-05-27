
+++
title = "Active - Active"
weight = 4
chapter = false
pre = "<b></b>"
+++


A solução de arquitetura de aplicações multirregional ajuda a demonstrar uma aplicação tolerante a falhas com facilidade de failover para uma região de backup. Essa solução implanta uma solução que tira proveito da Replicação entre regiões do Amazon Simple Storage Service (Amazon S3) e das Tabelas globais do Amazon DynamoDB para replicar dados de aplicativos de forma assíncrona entre as Regiões primárias e secundárias da AWS. Um aplicativo web de compartilhamento de fotos também é implantado em cada região para servir como uma demonstração visual das camadas de back-end da solução e para verificar se o failover regional está funcionando.


![AWS Solutions - Arquitetura de Aplicativos em várias regiões](/images/aws-sol-multi-region-application.png)


{{< button href="https://docs.aws.amazon.com/solutions/latest/multi-region-application-architecture/welcome.html" >}}Visualizar Guia de Implementação{{< /button >}}




{{< button href="https://github.com/awslabs/multi-region-application-architecture"icon="fab fa-github" >}}GitHub Repo{{< /button >}}


---
**Referências:**
- [AWS Solutions - Arquitetura de aplicativos em várias regiões](https://aws.amazon.com/pt/solutions/implementations/multi-region-application-architecture/)
