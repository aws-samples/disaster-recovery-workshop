---
title : "Amazon API Gateway - Backup & Restore"
menuTitle : "Backup & Restore"
tags:
  - Backup & Restore
awsServices:
  - Amazon API Gateway
---

Amazon API Gateway offers different ways for you to keep backing up your apis and restore the later.

It supports working with infrastructure as code through the Serverless Application Model (SAM), through AWS CloudFormation itself, or also through standard formats for defining apis such as OpenAPI and Swagger (for REST apis).

The references include the procedures for exporting APIs according to the type used:

*   REST API
*   HTTP API

For the recreation/replacement of the API, you can use the API definition import engine in the OpenAPI standard.

For WebSocket apis, the use of infrastructure as code is the best alternative to maintain configurations and replicate them later.

***

**References:**

*   [Exporting a REST api from API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-export-api.html)
*   [Exporting an API Gateway HTTP Api](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-export.html)
*   [Defining an API Using the Serverless Application Model (SAM)](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-resource-api.html)
