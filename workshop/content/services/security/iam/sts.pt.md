---
title: "AWS Security Token Service"
menutitle: "AWS STS"
weight: 1
chapter: true
pre: "<b></b>"
---

## Endpoints Regionais
Por padrão, o AWS Security Token Service (AWS STS) está disponível como um serviço global, e todas as solicitações do AWS STS vão para um único endpoint em https://sts.amazonaws.com. A AWS recomenda o uso de endpoints regionais do AWS STS, em vez do endpoint global, para reduzir a latência, incorporar redundância e aumentar a validade do token de sessão.

Reduzir a latência – ao fazer suas chamadas do AWS STS para um endpoint mais próximo de seus serviços e aplicativos, você pode acessar os serviços do AWS STS com menor latência e tempos de resposta mais curtos.

Incorporar redundância – você pode adicionar código ao aplicativo que muda suas chamadas de API do AWS STS para uma região diferente. Isso garante que, se a primeira região parar de responder, o aplicativo continuará a funcionar. Essa redundância não é automática, você deve criar a funcionalidade em seu código.

Aumentar a validade do token de sessão – tokens de sessão de endpoints regionais do AWS STS são válidos em todas as regiões da AWS. Tokens de sessão do endpoint global do STS são válidos apenas em regiões da AWS que são habilitadas por padrão. Se pretende habilitar uma nova região para sua conta, você pode usar tokens de sessão de endpoints regionais do STS. Se optar por usar o endpoint global, você deverá alterar a compatibilidade de regiões de tokens de sessão do STS para o endpoint global. Isso garante que os tokens sejam válidos em todas as regiões da AWS.


{{% notice tip %}}
<i class="fas fa-lightbulb"></i>
Veja a tabela de [endpoints regionais do STS](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html#id_credentials_region-endpoints) e certifique-se que sua aplicação está usando ao menos dois endpoints regionais.
{{% /notice %}}

---
## VPC Endpoints para o AWS STS
Se você usa a Amazon Virtual Private Cloud (Amazon VPC) para hospedar seus recursos da AWS, pode estabelecer uma conexão privada entre a VPC e o AWS STS. Você pode usar essa conexão para habilitar o AWS STS a se comunicar com os seus recursos na VPC sem passar pela Internet pública.

A Amazon VPC é um serviço da AWS que você pode usar para executar os recursos da AWS em uma rede virtual definida por você. Com a VPC, você tem controle sobre as configurações de rede, como o intervalo de endereços IP, sub-redes, tabelas de rotas e gateways de rede. Para conectar sua VPC ao AWS STS, você define um VPC endpoint de interface para o AWS STS. O endpoint fornece uma conectividade confiável e escalável ao AWS STS sem a necessidade de um gateway da Internet, de uma instância de conversão de endereço de rede (NAT) ou de uma conexão VPN.


---
**Referências:**
- [Using AWS Security Token Service in Your .NET Application](https://www.youtube.com/watch?v=9e2DdGT5mZg)