---
title: "Pilares para Workloads Resilientes"
menuTitle: "Workloads Resilientes"
weight: 3
chapter: false
pre: "<b></b>"
type: documentation
---

Existem 3 pilares cruciais para que *workloads* executados no ambiente de *Cloud* sejam resilientes desde sua concepção. Estes pilares baseam as decisões de arquitetura que habilitam a movimentação do *workload* de maneira flexível e segura:
<br><br>

|Infrastructure-as-code|Data Movement | Application Architecture |
|------------------------|---------------|--------------------------|
| Garantir que sua arquitetura seja replicável de forma automatizada, tornando possível a criação e recriação deste ambiente em outra localização, além de dar visibilidade às mudanças através de ferramentas de versionamento de código, como o [Git](https://git-scm.com/) |  Dados tem gravidade. Isto significa que a tendência é que todos os serviços necessários para a manipulação destes dados estejam o mais próximo possível do seu repositório. Deve-se considerar questões como latência, volumetria, custos e forma de acesso a estes dados, planejando formas específicas de movimentação caso necessário. | Conceber *workloads* que garantam o baixo acoplamento entre os componentes, não mantenham estado, se comuniquem de forma assíncrona e garantam a persistência e integridade dos dados em repositórios externos. Desta forma, é possível que sua aplicação se movimente sem impactos aos usuários ou aos dados sendo manipulados |
| **Referências:**<br> [Infrastructure-as-code](https://docs.aws.amazon.com/whitepapers/latest/introduction-devops-aws/infrastructure-as-code.html), [Gerenciamento e Governança]({{< ref "/services/management" >}} "Gerenciamento e Governança") | **Referências:**<br>[Armazenamento]({{< ref "/services/storage" >}} "Armazenamento"), [Banco de dados]({{< ref "/services/databases" >}} "Banco de dados"), [Redes e entrega de conteúdo]({{< ref "/services/networking" >}} "Redes e entrega de conteúdo")|**Referências:**<br> [Modern Application Architectures](https://www.youtube.com/watch?v=wD_uaqCu5Mc), [Computação]({{< ref "/services/compute" >}} "Computacao"), [Contêineres]({{< ref "/services/containers" >}} "Containers"), [Integração de aplicativos]({{< ref "/services/networking" >}} "Integração de aplicativos")|
