---
title : "Amazon API Gateway - Warm Standby"
menuTitle: "Warm Standby"
tags:
  - Warm Standby
awsServices:
  - Amazon API Gateway
---

Em se tratando de apis externas acessíveis pela internet, uma configuração possível que suporta um cenário warm standby para suas apis em diferentes regiões é a seguinte vista na figura abaixo.

Suportado pelas políticas de roteamento do Route53, é possível direcionar o tráfego de suas apis para uma infraestrutura em uma região, e através da funcionalidade de health check, o serviço percebe a ocorrência de uma indisponibilidade e faz o chaveamento de entradas de DNS para direcionar o tráfego para outra região onde suas apis já se encontram definidas e implantadas. 

![API Gateway Multi-region](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2017/11/08/active-active-architecture.png)



---
**Referências:**
- [Building a multi region serverless with Amazon API Gateway and AWS Lambda](https://aws.amazon.com/blogs/compute/building-a-multi-region-serverless-application-with-amazon-api-gateway-and-aws-lambda/)