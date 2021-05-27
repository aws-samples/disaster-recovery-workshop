---
title: "Amazon VPC"
menutitle: "Amazon VPC"
weight: 3
chapter: true
pre: "<b></b>"
awsServices:
 - "Amazon VPC"
type: Documentation
---

A Amazon Virtual Private Cloud (Amazon VPC) permite executar recursos da AWS em uma rede virtual definida por você. Essa rede virtual se assemelha a uma rede tradicional que você operaria no seu datacenter, com os benefícios de usar a infraestrutura dimensionável da AWS.

### Conceitos da Amazon VPC
A Amazon VPC é a camada de rede do Amazon EC2. Se você não tem experiência com o Amazon EC2, consulte O que é o Amazon EC2? no Guia do usuário do Amazon EC2 para instâncias do Linux para obter uma breve visão geral.

Veja a seguir os principais conceitos das VPCs:

* **Virtual Private Cloud (VPC):** uma rede virtual dedicada à sua conta da AWS.

* **Sub-rede:** um intervalo de endereços IP na VPC.

* **Tabela de rotas:** um conjunto de regras, chamadas de rotas, que são usadas para determinar para onde o tráfego de rede será direcionado.

* **Gateway da Internet:** um gateway que você anexa à VPC para permitir a comunicação entre recursos na VPC e a Internet.

* **VPC endpoint:** permite que você conecte de forma privada a VPC aos serviços da AWS compatíveis e aos serviços do VPC endpoint desenvolvidos pelo PrivateLink sem exigir um gateway da Internet, um dispositivo NAT, uma conexão VPN ou uma conexão do AWS Direct Connect. As instâncias na sua VPC não exigem que endereços IP públicos se comuniquem com recursos no serviço. O tráfego entre a sua VPC e os outros serviços não deixa a rede da Amazon.

---
**Referências**
- [Amazon VPC](https://docs.aws.amazon.com/pt_br/vpc/latest/userguide/what-is-amazon-vpc.html)