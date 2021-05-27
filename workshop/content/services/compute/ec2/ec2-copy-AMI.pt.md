---
title: "EC2 - Copiando uma AMI para outra Região"
menutitle: "Cópia de AMIs"
weight: 1
chapter: true
pre: "<b></b>"
awsServices:
 - Amazon EC2
tags: 
 - Backup & Restore
---

Você pode copiar uma Imagem de Máquina da Amazon (AMI) na ou para outra região da AWS usando o Console de gerenciamento da AWS, a AWS Command Line Interface ou SDKs, ou a API do Amazon EC2, sendo que todos oferecem suporte à ação CopyImage. Você pode copiar as AMIs baseadas no Amazon EBS e as AMIs com armazenamento de instâncias. Você pode copiar AMIs com snapshots criptografados e também alterar o status de criptografia durante o processo de cópia.
Copiar uma AMI de origem resulta em uma AMI de destino idêntica, mas com seu próprio identificador exclusivo. No caso de uma AMI com Amazon EBS, cada um de seus snapshots de suporte é, por padrão, copiado para um snapshot de destino idêntico, mas distinto. (As únicas exceções são quando você escolhe criptografar o snapshot ou criptografá-lo novamente.) Você pode alterar ou cancelar o registro da AMI de origem sem afetar a AMI de destino. O inverso também é verdadeiro.
Não há cobrança para copiar uma AMI. Mas aplicam-se as taxas padrão de transferência de dados e armazenamento. Se copiar uma AMI baseada em EBS, você será cobrado pelo armazenamento de snapshots adicionais do EBS.

O diagrama a seguir mostra as relações entre uma AMI de origem e duas AMIs copiadas em regiões diferentes, assim como as instâncias do EC2 executadas de cada uma. Ao executar uma instância a partir de uma AMI, ela residirá na mesma região em que a AMI reside. Se você fizer alterações à AMI de origem e quiser que essas alterações sejam refletidas nas AMIs das regiões de destino, deve recopiar a AMI de origem nas regiões de destino.

![AMI Copy](/images/ec2-ami-copy.png)

Ao copiar pela primeira vez uma AMI com armazenamento de instâncias para uma região, criaremos um bucket do Amazon S3 para as AMIs copiadas para essa região. Todas as AMIs com armazenamento de instâncias que você copiar para essa região serão armazenadas nesse bucket. Os nomes do bucket têm o seguinte formato: *amis-for-account-in-region-hash*. Por exemplo: *amis-for-123456789012-in-us-east-2-yhjmxvp6*.

Veja o procedimento nesse [blog post](https://aws.amazon.com/blogs/aws/ec2-ami-copy-between-regions/).


---
**Referências**
- [Amazon EC2 - Copiando uma AMI](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/CopyingAMIs.html)
- [Amazon EC2 - Cópia de AMI para outros regiões](https://aws.amazon.com/pt/premiumsupport/knowledge-center/copy-ami-region/)
