---
title: "Amazon Route53"
menutitle: "Amazon Route53"
weight: 3
chapter: true
pre: "<b></b>"
awsServices:
 - "Amazon Route53"
type: Documentation
---


O Amazon Route 53 é um serviço web do Domain Name System (DNS) altamente disponível e dimensionável. Você pode usar o Route 53 para executar três funções principais em qualquer combinação: registro de domínios, roteamento de DNS e verificação de integridade. Se você optar por usar o Route 53 para todas as três funções, execute as etapas nesta ordem:

**1. Registrar nomes de domínio**

Seu site precisa de um nome, como exemplo.com. O Route 53 permite que você registre um nome para seu site ou aplicativo web, conhecido como um nome de domínio.


**2. Rotear tráfego de Internet para os recursos do seu domínio**

Quando um usuário abre um navegador da web e informa seu nome de domínio (example.com) ou nome de subdomínio (acme.example.com) na barra de endereços, o Route 53 ajuda a conectar o navegador com o site ou aplicativo web.

**3. Verificar a integridade de seus recursos (*Health Checks*)**

O Route 53 envia solicitações automáticas através da Internet a um recurso, como um servidor web, para verificar se está acessível, disponível e funcional. Você também pode optar por receber notificações quando um recurso se tornar indisponível e optar por desviar o tráfego da Internet dos recursos não íntegros.


---
**Referências**
- [Amazon Route53](https://docs.aws.amazon.com/pt_br/Route53/latest/DeveloperGuide/Welcome.html)