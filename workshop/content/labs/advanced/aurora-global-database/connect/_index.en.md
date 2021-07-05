---
title: "Connect to Session Manager Workstation"
menutitle: "Using Session Manager"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Active-Active]
type: Lab
awsServices:
 - Amazon Aurora
---


To interact with the Aurora DB cluster, you will use a <a href="https://aws.amazon.com/ec2/" target="_blank">Amazon EC2</a> Linux that acts as a workstation to interact with AWS resources in labs. All necessary software packages and scripts have been installed and configured on this EC2 instance for you. To ensure a unified experience, you'll be interacting with this workstation using  <a href="https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html" target="_blank">AWS Systems Manager Session Manager</a>. With Session Manager you can interact with your workstation directly from the management console, without the need to install any software on your own devices.

## 1. Connect your workstation instance

<!-- Se você nunca utilizou o Session Manager, click em **Get Started with Systems Manager** 

![Get Started Session](1-getstarted2.png)


Clique em **Create**:
![Get Started Session](1-create.png.png)

Selecione a opção **Host Management**:
![Get Started Session](1-hostbase.png.png)
 -->

Open the <a href="https://console.aws.amazon.com/systems-manager/session-manager" target="_blank"> Systems Manager: Session Manager Service Console </a>. Click the button **Start session**.

{{% notice warning%}}
Make sure you have selected the correct region.
{{% /notice%}}

![Start Session](/images/aurora-1.start-session.png?raw=true)

Select an EC2 instance to establish a session. The workstation is called `auroralab-mysql-workstation`, select it and click **Start session**.

![Connect Instance](/images/aurora-1-connect-session.png?raw=true)

You should see a terminal screen and a prompt when connected to the workstation. Enter the following commands to ensure the connection was successful:

```shell
sudo su -l ubuntu
```

{{% notice warning%}}
Linux User Account

By default, Session Manager connects using the user account **ssm-user**. With the above command, you are changing the user account to the user's account **untu**, which has the correct settings required for laboratories. You can always verify the current user's account by typing `whoami`, this command will show the current user's account.
{{% /notice%}}

If you encounter errors while accessing subsequent lab commands, it's likely that the user's account has not been changed with the above command.

![Terminal Connected](/images/aurora-1-terminal-sudo.png?raw=true)

## 2. Check the lab environment

Let's make sure your workstation has been set up correctly. Type the following command at the Session Manager command line:

```shell
tail -n1 /debug.log
```

You should see the output: `* bootstrap complete, rebooting`If that's not the output you see, please wait a few more minutes and try again.

After verifying that the environment is set up correctly, you can proceed to the next lab.
