## Recuperação de Desastres na AWS

Languages: [EN](README.md)

O foco desse conteúdo são os padrões de confiabilidade e como aplicá-lo às suas soluções usando os serviços da AWS.
Este material é baseado em [Pilar de confiabilidade](https://d1.awsstatic.com/whitepapers/architecture/AWS-Reliability-Pillar.pdf?ref=reliability-refarch) do [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/?ref=reliability-refarch).
Esse material fornece um conjunto de mecanismos para ajudar os clientes a aplicar as melhores práticas no projeto, entrega e manutenção de ambientes da Amazon Web Services (AWS).
Ao adotar as abordagens arquiteturais neste conteúdo, você criará soluções que possuem componentes resilientes e processos de recuperação comprovados.

Esse conteúdo é destinado aos profissionais de tecnologia, como Chief Technology Officers (CTOs), arquitetos de soluções, desenvolvedores e membros da equipe de operações.
Depois de ler isso, você entenderá as melhores práticas e estratégias da AWS para usar ao projetar arquiteturas de nuvem para confiabilidade.

![DR strategies](workshop/static/images/dr-multi-region.png)

## Primeiros passos

Visite o workshop sobre este URL: [recuperação de desastres.workshop.aws](https://disaster-recovery.workshop.aws/)

## Guia do desenvolvedor

Este workshop é construído usando markdown como um site HTML estático usando [hugo](http://gohugo.io).

```bash
$ brew install hugo
```

Você encontrará o conteúdo da oficina no [workshop/](workshop/) diretório.

Você pode iniciar um servidor de desenvolvimento local executando:

```bash
$ cd workshop
$ hugo server -D
$ open http://localhost:1313/
```

## Quer contribuir?

Veja [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) para obter mais informações.

## Resumo da licença

A documentação é disponibilizada sob a Creative Commons Attribution-ShareAlike 4.0 International License. Consulte o arquivo LICENSE.

O código de exemplo nesta documentação é disponibilizado sob a licença MIT-0. Consulte o arquivo LICENSE-SAMPLECODE.
