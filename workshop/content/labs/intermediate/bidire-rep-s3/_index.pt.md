---
title: "Amazon S3 - Replicação Bidirecional entre Regiões (CRR) "
menutitle: "S3 - Replicação Bidirecional"
chapter: false
tags:
  - Active-Active
type: Lab
awsServices: 
  - Amazon S3
---

### Objetivo

Esse exercício irá mostrar os passos para o uso da replicação de objetos através de regiões AWS em bucket do Amazon S3 (*Cross region replication*). Caso queira mais informações [clique aqui]({{< ref "/services/storage/s3/s3-crr" >}} "Amazon S3").

**Ao final desse exercício, você será capaz de:**
- Executar a replicação automática de objetos em buckets do Amazon S3
- Automatizar a recuperação de desastres (DR) de aplicações que utilizam objetos no Amazon S3
- Buscar logs no CloudTrail para melhorar o entendimento do funcionamento da replicação entre regiões.

**Tempo de Execução Estimado:** 20 minutos

**Custo Aproximado**: 1 USD

### Introdução

Esse laboratório irá guiá-lo para aumentar a resiliência das aplicações que dependem de objetos no Amazon S3. A funcionalidade de [replicação do Amazon S3 (CRR)](https://docs.aws.amazon.com/pt_br/AmazonS3/latest/dev/replication.html), permite a cópia assícrona para outra região da AWS.

![S3 Lab Architecture](/images/s3-bi-crr.png?width=550px")

### Execução
{{< tabs name="labs_types" >}} 
{{< tab name="AWS Console" include="console" />}} 
{{< /tabs >}}

---

#### Autor

* Seth Eliot, Principal Reliability Solutions Architect, AWS Well-Architected


**Fonte:** [Amazon S3 - Implementing BI-Directional Cross-Region Replication (CRR)](https://wellarchitectedlabs.com/reliability/200_labs/200_bidirectional_replication_for_s3/)


