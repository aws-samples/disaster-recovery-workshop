---
title: "Create a Workspace"
chapter: false
weight: 14
---

The AWS Cloud9 Workspace is a cloud-based integrated development environment (IDE) that will be used for some exercises, other exercises will be run through the AWS CloudShell or the AWS Console. Thus check in the first steps of the chosen exercise if it will be necessary to create a Workspace.

{{% notice warning %}}
The Cloud9 workspace should be built by an IAM user with Administrator privileges,
not the root account user. Please ensure you are logged in as an IAM user, not the root
account user.
{{% /notice %}}

{{% notice tip %}}
Ad blockers, javascript disablers, and tracking blockers should be disabled for
the cloud9 domain, or connecting to the workspace might be impacted.
Cloud9 requires third-party-cookies. You can whitelist the [specific domains]( https://docs.aws.amazon.com/cloud9/latest/user-guide/troubleshooting.html#troubleshooting-env-loading).
{{% /notice %}}

### Launch Cloud9 in your closest region:
{{< tabs name="Region" >}}
{{{< tab name="N. Virginia" include="us-east-1.md" />}}
{{{< tab name="N. California" include="us-west-1" />}}
{{{< tab name="Oregon" include="us-west-2.md" />}}
{{{< tab name="Ireland" include="eu-west-1.md" />}}
{{{< tab name="Ohio" include="us-east-2.md" />}}
{{{< tab name="Singapore" include="ap-southeast-1.md" />}}
{{{< tab name="Sydney" include="ap-southeast-2.md" />}}
{{{< tab name="Sao Paulo" include="sa-east-1.md" />}}
{{< /tabs >}}

- Select **Create environment**
- Name it **workshop**, click Next.
- In the Environment Settings  make sure the option **Create a new EC2 instance for environment (direct access)** is selected.
- Choose **t3.small** for instance type, take all default values and click **Next Step**
- Review the details, and click **Create environment** (this will take a few minutes)

- When the Cloud9 console is shown, customize the environment by closing the **welcome tab** and the **AWS Toolkit** tab, **lower work area**, and opening a new **terminal** tab in the main work area:
![c9before](/images/c9before.png)

- Your workspace should now look like this:
![c9after](/images/c9after.png)


{{% notice tip %}}
If you like this theme, you can choose it yourself by selecting **View / Themes / Solarized / Solarized Dark**
in the Cloud9 workspace menu.

{{% /notice %}}

