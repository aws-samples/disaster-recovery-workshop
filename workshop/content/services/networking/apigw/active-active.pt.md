---
title : "Amazon API Gateway - Active-Active"
menuTitle : "Active-Active"
date : 2020-12-17T17:41:47-03:00
weight : 4
tags:
  - Active-Active
awsServices:
  - Amazon API Gateway
---

Em se tratando de apis externas acessíveis pela internet, uma configuração possível que suporta um cenário ativo-ativo para suas apis em diferentes regiões pode ser conforme a figura abaixo.

Suportado pelas políticas de roteamento do Route53, é possível direcionar o tráfego de suas apis para mais de uma infraestrutura em diferentes regiões, e através das políticas de roteamento por peso, o serviço permite balancear as requisições conforme a capacidade da infraestrutura de cada região.  

![API Gateway Multi-region](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2017/11/08/route-53-healthcheck.png)



---
**Referências:**
- [Building a multi region serverless with Amazon API Gateway and AWS Lambda](https://aws.amazon.com/blogs/compute/building-a-multi-region-serverless-application-with-amazon-api-gateway-and-aws-lambda/)