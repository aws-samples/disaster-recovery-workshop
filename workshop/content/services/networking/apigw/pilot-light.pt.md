---
title : "Amazon API Gateway - Pilot Light"
menuTitle : "Pilot Light"
tags:
  - Pilot Light
awsServices:
  - Amazon API Gateway
---

Para manter as configurações de suas apis do Amazon API Gateway replicadas para uma outra região de modo a permitir rapidamente subir um novo ambiente temos os seguintes passos em alto nível que podem ser seguidos:

1. Exportar a API do Amazon API Gateway em um arquivo Swagger ou equivalente usando funcionalidade disponível do próprio serviço.

2. Copiar o arquivo de definição para a região destino usando ferramentas padrão como CP, FTP, rsynch, etc.

3. Importar o arquivo para criar uma nova API no API Gateway na região de destino. Por se tratar de solução serverless, o simples fato de criar a API não incorre em custos se não houver tráfego na api.

