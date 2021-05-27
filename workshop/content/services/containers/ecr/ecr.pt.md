---
title: "Amazon ECR - Replicação de Imagens para Múltiplas Regiões"
menuTitle: "Replicação de Imagens"
weight: 1
chapter: false
pre: "<b></b>"
awsServices:
 - Amazon ECR
type: documentation
tags: 
 - Active-Active
---

Amazon Elastic Container Registry (Amazon ECR) é uma ferramenta AWS o serviço de registo de imagens de contentores que é seguro, escalável e fiável. Amazon ECR suporta repositórios de imagens de recipientes privados com permissões baseadas em recursos utilizando AWS IAM para que utilizadores específicos ou Amazon EC2 podem aceder a repositórios e imagens. Os programadores podem usar a sua CLI preferida para empurrar, puxar e gerir imagens Docker, imagens Open Container Initiative (OCI) e artefactos compatíveis com OCI.

A equipe de serviços de contêiner da AWS mantém um roteiro público no GitHub. Ele contém informações sobre no que as equipes estão trabalhando e permite que todos os clientes da AWS forneçam feedback diretamente. Para obter mais informações, consulte [Roteiro de contêineres da AWS](https://github.com/aws/containers-roadmap).

---

## Replicação de Imagens para Múltiplas Regiões


Considerando que a sua aplicação segue as recomendações do artigo acima, dentre elas, ter containers *stateless* e Infrastructure-as-Code. Você pode desenhar sua automação de CI/CD para que ela armazene as imagens dos seus containers após a criação da imagem em duas regiões, este [blog post](https://aws.amazon.com/blogs/containers/cross-region-replication-in-amazon-ecr-has-landed/) demonstra como é possível fazer isso conforme a imagem abaixo:

![ECR cross-region](/images/ecr-crr-scenario-1.png)


É importante ressaltar que mesmo tendo a imagem do seu container em duas regiões é de suma importância você garantir que seu codebase, principalmente seus scripts de deploy e infraestrutura como código, estejam disponíveis em caso de desastre em uma região pois você pode necessitar dos arquivos de especificação de deploy.

Você pode optar por manter a especificação da sua pipeline partindo do Amazon ECR e o seu cluster de Amazon ECS e/ou Amazon EKS em infrastructure-as-code e cria-la quando necessário, ou até mesmo já mantê-los provisionados na segunda região. Recomendamos que você teste e documente o processo de fail-over para avaliar se atende as suas necessidades de RPO/RTO e adaptar se necessário.

---

**Referências**
- [Amazon ECR - Private registry setting examples](https://docs.aws.amazon.com/pt_br/AmazonECR/latest/userguide/registry-settings-examples.html)