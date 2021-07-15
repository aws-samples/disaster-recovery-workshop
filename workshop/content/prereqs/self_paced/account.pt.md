---
title: "Criar uma conta AWS"
chapter: false
weight: 1
---

{{% notice warning %}}
Você deve ter um usuário com permissões para criar IAM roles em sua conta AWS.
{{% /notice %}}

Caso ainda não tenha uma conta AWS com acesso Administrador, [crie uma conta aqui.](https://aws.amazon.com/getting-started/)

Siga os passos abaixo usando um usuário IAM com acesso de Administrador na conta AWS:

1. Acesse a console do [IAM](https://console.aws.amazon.com/iam/home?#/users$new)

2. No lado esquerdo da tela, selecione **Users (Usuários)**, então clique em **Add User (Adicionar Usuário)**.

3. Crie um usuário, conforme a imagem abaixo:
![Criar Usuário](/images/iam-1-create-user.png)

1. Attach the AdministratorAccess IAM Policy, and click **Next: Tags**
![Attach Policy](/images/iam-2-attach-policy.png)

1. We will skip adding tags for the workshop. Click **Next: Review**

1. Click to create the new user:
![Confirm User](/images/iam-3-create-user.png)

1. Take note of the login URL and save it for future use:
![Login URL](/images/iam-4-save-url.png)

1. Click on this URL to go to the AWS Management Console login page, use the username and password for the `workshop` user you just created.

Once you have completed the step above, you can head straight to [**Create a Workspace**]({{% relref workspace %}})