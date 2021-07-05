
---
title: "ElastiCache - Backup & Restore"
menuTitle: "Backup & Restore"
date: 2020-12-14T21:49:53-03:00
weight: 1
tags:
 - Backup & Restore
awsServices:
 - Amazon ElastiCache
---

É possível fazer backup de clusters ElastiCache para Redis apenas. Clusters Memcached não possuem esta funcionalidade. Na configuração do cluster define-se a estratégia de backups diários automatizados com período de retenção máximo de 35 dias. Os backups utilizam a estratégia nativa do Redis, gerando arquivos .rdb que podem ser exportados para um bucket S3 e, posteriormente, portado para outra região (através do cross-region replication do S3, por exemplo) caso necessário.

Recomenda-se planejar a estratégia de backups definindo a janela de execução para o período com menor carga prevista no dia. Backups manuais durante o dia impactam a performance do cluster. Para aumentar a performance do backup, considere ajustar o parâmetro *reserved-memory-percent* para minimizar a paginação excessiva. Considere também executar backups à partir de *read-replicas*, minimzando a concorrência de gravação de dados no cluster.

Uma vez exportado o backup, é possível criar um novo cluster utilizando o arquivo .rdb gerado em qualquer região onde o Amazon ElastiCache esteja disponível. Uma vez restaurado o backup, o cluster Redis estará *warmed up* e pronto para receber requests.

**RPO:** 1 dia (backup automático).  Menor que 1 dia (backups manuais) - Performance, sobrecarga e tempo de execução devem ser considerados

**RTO:** Dependente do tamanho do backup

---
**Referências:**
- [Backup and Restore for ElastiCache for Redis](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/backups.html)
- [Improving Backup Performance](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/backups.html#backups-performance-improving)
- [Redis memory management](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/redis-memory-management.html)
- [S3 replication](https://docs.aws.amazon.com/AmazonS3/latest/dev/replication.html)