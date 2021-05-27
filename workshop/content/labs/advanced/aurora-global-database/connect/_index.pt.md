---
title: "Conecte-se ao Session Manager Workstation"
menutitle: "Usando o Session Manager"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Active-Active]
type: Lab
awsServices:
 - Amazon Aurora
---

Para interagir com o cluster de banco de dados Aurora, você usará um <a href="https://aws.amazon.com/ec2/" target="_blank">Amazon EC2</a> Linux que atua como uma estação de trabalho para interagir com os recursos da AWS nos laboratórios. Todos os pacotes de software e scripts necessários foram instalados e configurados nesta instância EC2 para você. Para garantir uma experiência unificada, você estará interagindo com esta estação de trabalho usando  <a href="https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html" target="_blank">AWS Systems Manager Session Manager</a>. Com Session Manager você pode interagir com sua estação de trabalho diretamente do console de gerenciamento, sem a necessidade de instalar nenhum software em seus próprios dispositivos. 

## 1. Conecte-se a instância do workstation

<!-- Se você nunca utilizou o Session Manager, click em **Get Started with Systems Manager** 

<span class="image">![Get Started Session](1-getstarted2.png)</span>


Clique em **Create**:
<span class="image">![Get Started Session](1-create.png.png)</span>

Selecione a opção **Host Management**:
<span class="image">![Get Started Session](1-hostbase.png.png)</span> -->



Abra o <a href="https://console.aws.amazon.com/systems-manager/session-manager" target="_blank"> Systems Manager: console de serviço do Session Manager </a>. Clique no botão **Start session**.

{{% notice warning %}}
Certifique-se que você selecionou a região correta. 
{{% /notice %}}

<span class="image">![Start Session](1.start-session.png?raw=true)</span>

Selecione uma instância EC2 para estabelecer uma sessão. A estação de trabalho é chamada `auroralab-mysql-workstation`, selecione-a e clique **Start session**.

<span class="image">![Connect Instance](1-connect-session.png?raw=true)</span>

Você deve ver uma tela de terminal e um prompt quando estiver conectado à estação de trabalho. Digite os seguintes comandos para garantir que a conexão foi bem-sucedida:

```shell
sudo su -l ubuntu
```
{{% notice warning %}}
Linux User Account

Por padrão, o Session Manager se conecta usando a conta de usuário **ssm-user**. Com o comando acima, você está alterando a conta do usuário para a conta do usuário **ubuntu**, que possui as configurações corretas necessárias para os laboratórios. Você sempre pode verificar a conta do usuário atual digitando ```whoami```, esse comando irá mostrar a conta do usuário atual.
{{% /notice %}}

Se você encontrar erros ao acessar os comandos dos laboratórios subsequentes, é provável que a conta do usuário não tenha sido alterada com o comando acima. 

<span class="image">![Terminal Connected](1-terminal-sudo.png?raw=true)</span>

## 2. Verifique o ambiente do laboratório

Vamos nos certificar de que sua estação de trabalho foi configurada corretamente. Digite o seguinte comando na linha de comando do Session Manager:

```shell
tail -n1 /debug.log
```

Você deve ver a saída: `* bootstrap complete, rebooting`, se essa não for a saída que você vê, por favor, espere mais alguns minutos e tente novamente.

Depois de verificar se o ambiente está configurado corretamente, você pode prosseguir para o próximo laboratório.
