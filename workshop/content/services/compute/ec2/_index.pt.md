---
title: "Amazon EC2"
menutitle: "Amazon EC2"
weight: 1
chapter: true
pre: "<b></b>"
---

O Amazon Elastic Compute Cloud (Amazon EC2) é um serviço Web que disponibiliza capacidade computacional segura e redimensionável na nuvem. Ele foi projetado para facilitar a computação em nuvem na escala da web para os desenvolvedores. A interface de serviço Web simples do Amazon EC2 permite que você obtenha e configure a capacidade sem muito esforço. Oferece um controle completo de seus recursos computacionais e permite que você trabalhe no ambiente computacional comprovado da Amazon.

## O que é AMI?

Uma Imagem de máquina da Amazon (AMI) é um modelo que contém uma configuração de software (por exemplo, sistema operacional, servidor de aplicativo e aplicativos). A partir de uma AMI, execute uma instância, que é uma cópia da AMI que roda como servidor virtual na nuvem. Você pode executar várias instâncias de uma AMI, conforme mostrado na figura a seguir.

![EC2 - AMI](/images/ec2-instances-ami.png)


Suas instâncias continuarão sendo executadas até que você as interrompa, hiberne ou encerre, ou até que elas falhem. Se uma instância falhar, você pode executar uma nova instância a partir da AMI.

## Tipos de AMI

Você pode selecionar uma AMI para uso com base nas seguintes características:

* Região (consulte [Regiões e zonas](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/using-regions-availability-zones.html))
* Sistema operacional (Windows, Linux e MacOS)
* Arquitetura (32 bits ou 64 bits)
* [Permissões de execução](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/ComponentsAMIs.html#launch-permissions)
* [Armazenamento para o dispositivo raiz](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/ComponentsAMIs.html#storage-for-the-root-device)




---
**Referências**
- [Amazon EC2 - Instâncias](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/ec2-instances-and-amis.html)
- [Amazon EC2 - Tipos de AMI](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/ComponentsAMIs.html)