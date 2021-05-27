---
title: "Amazon S3"
weight: 2
chapter: true
pre: "<b></b>"
awsServices: 
    - Amazon S3
---

O Amazon Simple Storage Service é armazenamento para a Internet. Ele foi projetado para facilitar a computação de escala na web para os desenvolvedores.

O Amazon S3 tem uma interface simples de serviços da web que você pode usar para armazenar e recuperar qualquer quantidade de dados, a qualquer momento, em qualquer lugar da web. Ela concede acesso a todos os desenvolvedores para a mesma infraestrutura altamente dimensionável, confiável, segura, rápida e econômica que a Amazon utiliza para rodar a sua própria rede global de sites da web. O serviço visa maximizar os benefícios de escala e poder passar esses benefícios para os desenvolvedores.

## Modelo de consistência de dados do Amazon S3

O Amazon S3 oferece uma forte consistência de leitura após gravação para PUTs e DELETEs de objetos no bucket do Amazon S3 em todas as regiões da AWS. Isso se aplica a ambas as gravações em novos objetos, bem como PUTs que sobrescrevem objetos existentes e DELETEs. Além disso, as operações de leitura no Amazon S3 Select, Listas de controle de acesso do Amazon S3, Tags de objeto do Amazon S3 e metadados de objeto (por exemplo, objeto HEAD) são fortemente consistentes.

As atualizações em uma única chave são atômicas. Por exemplo, se você executar PUT em uma chave existente de um thread e executar um GET na mesma chave a partir de um segundo thread simultaneamente, você obterá os dados antigos ou os novos dados, mas nunca dados parciais ou corrompidos.

O Amazon S3 atinge alta disponibilidade replicando dados entre vários servidores nos datacenters da AWS. Se uma solicitação PUT for bem-sucedida, os dados serão armazenados com segurança. Qualquer leitura (GET ou LIST) iniciada após o recebimento de uma resposta PUT bem-sucedida retornará os dados escritos pelo PUT. Veja alguns exemplos desse comportamento:

* Um processo grava um novo objeto no Amazon S3 e imediatamente lista as chaves em seu bucket. O novo objeto aparecerá na lista.

* Um processo substitui um objeto existente e imediatamente tenta lê-lo. O Amazon S3 retornará os novos dados.

* Um processo exclui um objeto existente e imediatamente tenta lê-lo. O Amazon S3 não retornará dados conforme o objeto foi excluído.

* Um processo exclui um objeto existente e imediatamente lista as chaves em seu bucket. O objeto não aparecerá na listagem.

{{% notice note %}}

O Amazon S3 não oferece suporte ao bloqueio de objetos para escritores simultâneos. Se duas solicitações PUT forem realizadas simultaneamente na mesma chave, a solicitação com o time stamp mais recente será a escolhida. Se isso for um problema, você precisará criar um mecanismo de bloqueio de objetos em seu aplicativo.
{{% /notice %}}

{{% notice note %}}
As atualizações são baseadas em chave. Não há possibilidade de realizar atualizações atômicas entre chaves. Por exemplo, você não pode tornar a atualização de uma chave dependente da atualização de outra chave a menos que você desenvolva essa funcionalidade em seu aplicativo.

{{% /notice %}}

---
**Referências:**
- [S3 - Modelo de Consistência](https://docs.aws.amazon.com/pt_br/AmazonS3/latest/dev/Introduction.html#ConsistencyModel)
- [LAB: S3 - Replicação Bidirecional]({{< ref "bidire-rep-s3" >}})
