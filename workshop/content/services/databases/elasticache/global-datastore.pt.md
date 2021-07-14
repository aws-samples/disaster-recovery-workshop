---
title: "ElastiCache - Global Datastore"
menuTitle: "Global Datastore"
tags:
    - Warm Standby
awsServices:
 - Amazon ElastiCache
---


A funcionalidade Global Datastore para Redis possibilita que você trabalhe com a replicação entre regiões totalmente gerenciada e de forma rápida, confiável e segura. Cada Global Datastore é uma coleção de um ou mais cluster que replicam dados entre si. Para utilizar a replicação cross-region, é necessário ter pelo menos 2 clusters (**Primary** e **Secondary**, ou **Active** e **Passive**, respectivamente) configurados. O cluster primário é read/write, enquanto que o cluster **secundário** é apenas read e deve estar, obrigatoriamente, em **outra região**. 

Para a estratégia de Disaster Recovery, caso o cluster primário apresente sinais de degradação, um cluster secundário pode ser promovido a cluster primário. O diagrama abaixo ilustra seu funcionamento:

![Global Datastore](/images/Global-DataStore.png)

{{< youtube xx6X5s9uLBs >}}

**RTO:** 1 minuto

**RPO:** Geralmente menor que 1 segundo

---
**Referências:**
- [Global Datastore - Documentação oficial](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Redis-Global-Datastore.html)
- [Building a Global Solution with Amazon elastiCache for Redis Global Datastore](https://www.youtube.com/watch?v=xx6X5s9uLBs)