---
title: "Amazon VPC - Peering"
menutitle: "VPC Peering"
weight: 3
chapter: true
pre: "<b></b>"
awsServices:
 - "Amazon VPC"
type: Documentation
---

Uma conexão de emparelhamento de VPC é uma conexão de rede entre duas VPCs que permite direcionar o tráfego entre elas usando endereços IPv4 ou IPv6 privados. Instâncias em qualquer VPC podem se comunicar umas com as outras como se estivessem na mesma rede. É possível criar uma conexão de emparelhamento entre suas próprias VPCs ou com uma VPC de outra conta da AWS. As VPCs podem estar em regiões diferentes (também conhecidas como conexão de emparelhamento de VPC entre regiões).

![VPC Peering Diagram](/images/vpc-peering-diagram.png)
        
A AWS utiliza a infraestrutura existente da VPC para criar uma conexão emparelhada entre VPCs; ela não é um gateway e nem uma conexão VPN e não depende de hardware físico externo. Não há um ponto único de falha de comunicação ou um gargalo de largura de banda.

Uma conexão de emparelhamento de VPC ajuda você a facilitar a transferência de dados. Por exemplo, se houver mais de uma conta da AWS, você pode emparelhar as VPCs entre essas contas para criar uma rede de compartilhamento de arquivos. Você também pode usar uma conexão de emparelhamento de VPC para permitir que outras VPCs acessem os recursos que você tem em uma de suas VPCs.

Você pode estabelecer relações de emparelhamento entre VPCs entre diferentes regiões da AWS (também chamadas de emparelhamento de VPCs inter-região). Isso permite que os recursos da VPC, incluindo instâncias do EC2, bancos de dados do Amazon RDS e funções do Lambda que são executados em diferentes regiões da AWS se comuniquem entre eles utilizando endereços IP privados, sem exigir gateways, conexões VPN ou equipamentos de rede separados. O tráfego permanece no espaço IP privado. Todo o tráfego inter-região é criptografado sem nenhum ponto único de falha ou gargalo na largura de banda. O tráfego sempre fica no backbone global da AWS e nunca atravessa a internet pública, o que reduz ameaças, como violações comuns e ataques de DDoS. O emparelhamento de VPCs intra-região fornece uma forma simples e econômica de compartilhar recursos entre regiões ou replicar dados para redundância geográfica.

---
**Referências**
- [Amazon VPC Peering](https://docs.aws.amazon.com/pt_br/vpc/latest/peering/what-is-vpc-peering.html)