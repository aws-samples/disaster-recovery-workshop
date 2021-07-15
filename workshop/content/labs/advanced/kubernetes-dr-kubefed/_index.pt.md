---
title: "EKS - Ambiente de DR utilizando Kubefed"
menutitle: "EKS - Kubefed"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Active-Passive]
type: Lab
awsServices:
 - Amazon EKS
---

{{% notice warning %}}
**Observação:** Na data de publicação deste mini lab, a versão atual do KubeFed não está em estado de release e é recomendada apenas para fins de teste. Verifique o [status do KubeFed](https://github.com/kubernetes-sigs/kubefed) se você planeja usá-lo. 
{{% /notice %}}

{{% notice tip %}}
**Nota:** Existem outros métodos para o uso de EKS em um cenário de múltiplas regiões. Para mais detalhes acesse: 
[Using GitOps and AWS EKS to Manage HA & DR](https://weaveworks-gitops.awsworkshop.io/25_workshop_2_ha-dr.html)

{{% /notice %}}

### Objetivo

Esse exercício irá mostrar os passos para configurar e utilizar o [KubeFed](https://github.com/kubernetes-sigs/kubefed) que permite que você coordene a configuração de vários clusters Kubernetes de um único conjunto de APIs em um cluster, podendo ser utilizado em um cenário de multi região.

**Ao final desse exercício, você será capaz de:**

- Configurar o KubeFed para coordenar a configuração de vários clusters Kubernetes
- Fazer o deploy de uma aplicação que será criada em dois clusters Kubernetes em regiões destintas
- Utilizar o Route53 para configurar resolução de DNS com healthcheck para as duas regiões

**Tempo de Execução Estimado:** 1 hora

**Custo Aproximado**: 10 USD

### Visão Geral da Solução

![KubeFed - Federated clusters](/images/kubefed-arch.png)

### Execução

{{% notice info %}}
Caso ainda não tenha criado o ambiente no AWS Cloud9, comece criando seu workspace. [Clique aqui](pt/../../../../prereqs/workspace/workspace).
{{% /notice %}}

{{% notice note %}}
Para esse exercício utilize **N. Virginia (us-east-1)** como a região principal e a **Ohio (us-east-2)** como a região secundária.
{{% /notice %}}

{{< tabs name="labs_types" >}}  
{{< tab name="AWS Cloud9" include="cloud9" />}}
{{< /tabs >}}

### Conclusão

Com esse exercício, foi possivel entender como é possivel federar multiplos clusters em um único cluster para utilizar um único manifesto de Deployment e Service para replicar um POD em múltiplas regiões, neste caso na região **us-east-1** e **us-east-2**.