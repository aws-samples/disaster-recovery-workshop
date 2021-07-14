---
title : "Amazon API Gateway - Pilot Light"
menuTitle : "Pilot Light"
tags:
  - Pilot Light
awsServices:
  - Amazon API Gateway
---

To keep your Amazon API Gateway API settings replicated to another region to allow you to quickly move up a new environment, we have the following high-level steps that can be followed:

1.  Export the Amazon API Gateway API to a Swagger file or equivalent using the functionality available from the service itself.

2.  Copy the definition file to the destination region using standard tools such as CP, FTP, rsynch, etc.

3.  Import the file to create a new API in the API Gateway in the destination region. Because it is a serverless solution, the simple fact of creating the API does not incur costs if there is no traffic in the API.
