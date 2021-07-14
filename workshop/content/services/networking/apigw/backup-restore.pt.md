---
title : "Amazon API Gateway - Backup & Restore"
menuTitle : "Backup & Restore"
tags:
  - Backup & Restore
awsServices:
  - Amazon API Gateway
---

O Amazon API Gateway oferece diferentes formas para que você mantenha o backup de suas apis e faça o posterior restore. 

Ele oferece suporte para se trabalhar com infraestrutura como código através do SAM (Serverless Application Model), através do próprio AWS CloudFormation ou também através de formatos padrão de definição de apis como OpenAPI e Swagger (para apis REST).

Nas referências encontram-se os procedimentos de exportação de APIs conforme o tipo utilizado:

* API REST
* API HTTP

Para a recriação/substituição da API é possível utilizar o mecanismo de importação de definições de apis no padrão OpenAPI.

Para apis WebSocket, o uso de infraestrutura como código é a melhor alternativa para manter as configurações e replicá-las posteriormente.



---
**Referências:**
- [Exportando uma api REST do API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-export-api.html)
- [Exportando uma api HTTP do API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-export.html)
- [Definindo uma API usando o Serverless Application Model (SAM)](https://docs.aws.amazon.com/pt_br/serverless-application-model/latest/developerguide/sam-resource-api.html)