---
title: "AWS Secrets Manager - Replicação de Segredos"
menutitle: "Secrets Manager - Replicação"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Active-Active]
type: Lab
awsServices:
 - AWS Secrets Manager
---


### Introdução
O AWS Secrets Manager ajuda você a proteger os segredos necessários para acesso a aplicativos, serviços e recursos de TI. O serviço permite alternar, gerenciar e recuperar facilmente credenciais de banco de dados, chaves de APIs e outros segredos durante todo o seu ciclo de vida. Os usuários e os aplicativos recuperam os segredos com uma chamada às APIs do Secrets Manager, o que elimina a necessidade de codificar informações confidenciais em texto simples.  
É essencial estarmos preparados para eventuais falhas, tendo em vista a importância de uma chave guardada no AWS Secrets Manager, é crucial termos uma réplica dela across-region. É isso que o exercício a seguir ensinará, a replicação da chave secreta em mais de uma região.


### Visão geral da solução


![arc](images/architecture.png)


### Objetivo
Esse exercício irá mostrar o passo-a-passo para replicar a sua chave secreta (Segredo) no AWS Secrets Manager de uma região de origem para outra, dentro da AWS.

**Conhecimentos necessários:** 
- Uso básico do Console AWS

**Ao final desse exercício, você será capaz de:**
- Replicar chaves secretas do AWS Secrets Manager entre regiões AWS

**Tempo de Execução Estimado:** 20 minutos

**Custo Aproximado**: 1 USD


### Visão Geral 
Nesse exercícios serão executados os seguintes passos:

1. Replicar o Segredo para outra região 
2. Localizar o Segredo replicado

{{% notice note %}}
**OBS:** Nesse exercício a chave será replicada da região primária **N. Virginia** para a região secundária **Ohio**, porém esse mesmo procedimento poderá ser executado em outras região.
{{% /notice %}}



### Execução
{{< tabs name="labs_types" >}} 
{{< tab name="AWS Console" include="console" />}} 
{{< /tabs >}}

### Conclusão
Com esse exercício você aprendeu a replicar chaves secretas (Segredos) para outra região, utilizando o serviço AWS Secrets Manager. Caso algum incidente ocorra, você poderá utilizar as chaves secretas para acessar seus recursos em outra região AWS.

**Fonte:** [AWS Secrets Manager](https://aws.amazon.com/pt/secrets-manager/)

