---
title: "Amazon ElastiCache"
menutitle: "Amazon ElastiCache"
weight: 3
chapter: true
pre: "<b></b>"
---

O Amazon ElastiCache é um serviço gerenciado para in-memory database, compatível com [Memcached](https://memcached.org/) e [Redis](https://redis.io/). É utilizado primariamente como uma camada de cache para aplicações online em situações como acesso a bancos de dados relacionais e ao armazenamento temporário de dados de alta frequência, como dados de sessão de usuários por exemplo. É possível fazer um offload do banco de dados relacional, diminuindo a frequência de acesso serviços dependentes e possibilitando que microsserviços escalem sem a dependência de sincronização de dados.

É preciso planejar qual é a estratégia de cache mais adequada ao tipo de workload. Quando a estratégia definida envolve grandes volumes de dados, fazer um refresh completo destes dados se torna um desafio, podendo sobrecarregar os serviços dependentes e causando indisponibilidade do seu workload. Precisamos então distribuir este cache, de forma que ele consiga ter resiliência no armazenamento das informações e respeite as configurações de TTL definidas pelo workload. 

O Amazon ElastiCache possui duas opções: [Memcached](https://memcached.org/) e [Redis](https://redis.io/). Utilize o Memcached para implementações mais simples, quando seu requisito não envolver alta disponibilidade de dados. O Memcached não possui as funcionalidades de Replicação e failover automático.
{{% notice info %}}
O Redis deve ser utilizado como opção para Disaster Recovery por contar com as funcionalidades de Backup & Restore e do [Global Datastore](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Redis-Global-Datastore.html), lançado recentemente.
{{% /notice %}}

É possível implementar duas das quatro estratégias de Disaster Recovery: [Backup & Restore]({{< ref "tags/backup-restore" >}} "Backup & Restore") e [Warm Standby]({{< ref "tags/warm-standby" >}} "Warm Standby"). Discutiremos estas estratégias à seguir.

---
**Referências:**
- [Memcached ou Redis? Select Engine Guide](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/SelectEngine.html)
- [Fault Tolerance for ElastiCache](https://aws.amazon.com/premiumsupport/knowledge-center/fault-tolerance-elasticache/)
- [Amazon Builders' Library - Desafios e Estratégia de Cache](https://aws.amazon.com/pt/builders-library/caching-challenges-and-strategies/)
- [Amazon ElastiCache Redis Global Datastore](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Redis-Global-Datastore.html)
