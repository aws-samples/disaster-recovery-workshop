---
title: "Amazon Route 53 - Como o Trafégo de Internet é Roteado"
menutitle: "Como o trafégo é roteado"
weight: 2
chapter: true
pre: "<b></b>"
awsServices:
 - "Amazon Route 53"
type: Documentation
---

O diagrama abaixo detalha como ocorre o processo de resolução de URLs para endereços IP.

![Route53](/images/how-route-53-routes-traffic.png)

1. Um usuário abre um navegador da web, digita www.exemplo.com na barra de endereços e pressiona Enter.
   
2. A solicitação de www.exemplo.com é roteada para um resolvedor de DNS, que costuma ser gerenciado pelo provedor de serviço de Internet (ISP), como um provedor de Internet a cabo, um provedor de banda larga DSL ou uma rede corporativa.
   
3. O resolvedor de DNS do ISP encaminha a solicitação de www.exemplo.com para um servidor de nome raiz DNS.
   
4. O resolvedor de DNS encaminha a solicitação de www.exemplo.com novamente, desta vez para um dos servidores de nome TLD dos domínios .com. O servidor de nome dos domínios .com responde à solicitação com os nomes dos quatro servidores de nome do Route 53 associados ao domínio exemplo.com. 
O resolvedor DNS armazena em cache os quatro servidores de nome do Route 53. Na próxima vez que alguém acessar exemplo.com, o solucionador ignorará as etapas 3 e 4, pois ele já tem os servidores de nome para exemplo.com. Os servidores de nome são normalmente armazenados em cache por dois dias.

5. O resolvedor de DNS escolhe um servidor de nome do Route 53 e encaminha a solicitação de www.example.com para aquele servidor de nome.
   
6. O servidor de nome do Route 53 procura o registro www.exemplo.com na zona hospedada exemplo.com, obtém o valor associado, como o endereço IP de um servidor web, 192.0.2.44, e retorna o endereço IP para o resolvedor do DNS.
   
7. O resolvedor de DNS finalmente tem o endereço IP de que o usuário precisa. O resolvedor retorna o valor para o navegador da web.

{{% notice note %}}
O resolvedor de DNS também armazena em cache o endereço IP de exemplo.com por um período que você especificar. Assim, ele pode responder mais rapidamente na próxima vez que alguém acessar exemplo.com. Para obter mais informações, consulte [time to live (TTL)](https://docs.aws.amazon.com/pt_br/Route53/latest/DeveloperGuide/route-53-concepts.html#route-53-concepts-time-to-live).

{{% /notice %}}

8. O navegador da web envia uma solicitação de www.exemplo.com para um endereço IP que ele obteve do resolvedor de DNS. Este é o lugar onde está o seu conteúdo. Por exemplo, um servidor web que executa em uma instância do Amazon EC2 ou em um bucket do Amazon S3 que está configurado como um endpoint do site.

9. O servidor web ou outro recurso em 192.0.2.44 retorna a página da web de www.exemplo.com para o navegador da web e o navegador exibe a página.

---
**Referências:**
- [Amazon Route53 - Como o trafégo de internet é roteado](https://docs.aws.amazon.com/pt_br/Route53/latest/DeveloperGuide/welcome-dns-service.html)
