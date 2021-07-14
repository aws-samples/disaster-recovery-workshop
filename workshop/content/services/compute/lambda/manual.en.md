---
title: "AWS Lambda - Manual Copy"
menutitle: "Manual Copy"
weight: 4
awsServices: 
 - AWS Lambda
tags:
 - Backup & Restore
 - Pilot Light
---

It involves manually exporting, via console or aws-cli, its lambda functions and importing them into another region.
{{% notice warning%}}
Although possible, this strategy is very susceptible to manual errors. Using when you have few functions can be a strategy, albeit discouraged. Only the source code will be exported, so that the other settings such as Permissions, Memory, Timeout, Environment Variables, and other options have to be set manually. Also, always keep your source code in a versioning tool.
{{% /notice%}}

If you have the source code stored in a version manager, the export step below is not required.

#### Export (via Console)

To do this, go to the menu Services -> Lambda. Select Functions, click the desired function, actions -> Export Function. A popup will appear with the SAM File or Deployment Package Download options. Select Deployment Pack and the download will be downloaded. Note that the downloaded file is a zip file, but it doesn't have this extension. After downloading, rename the file to the .zip extension for later use.

#### Export (via CLI)

AWS CLI provides commands for locally listing and downloading all AWS Lambda functions. For example, to list the name of the existing roles in your account configured in `~/.aws/credentials`:

    aws lambda list-functions --region=us-east-2 | jq '[.Functions[] | select(.FunctionName) | .FunctionName]'

The above command only extracts the function name via jq. For details of a specific lambda function by name:

    aws lambda get-function --region=us-east-2 --function-name test | jq

The above command even returns the URL S3 from where the source code is located on the node. `Code.Location` from the returned json, in addition to memory settings, timeout, and others in `Configuration`.

#### Import (via Console)

Go back to the list of Functions in breadcrumb in the upper left corner. Select the new region in the combo in the upper right corner of the Console and click Create Role (if it doesn't exist). Name the role preferably with the same name as the region, taking care to ensure that other settings such as runtime, permissions, memory, and other options are the same as the source region. Once created, in the Source Code section:

*   Click Actions -> Upload a zip file.
*   Select the previously exported file
*   Click OK.

The source code will be imported into the current Lambda function. You can also import the source code from a.zip stored in an S3 bucket.

#### Import (via CLI)

You can also create AWS Lambda functions using the AWS CLI. To do this, use the method below:

    aws lambda create-function --function-name funcao-teste --zip-file fileb://meufonte.zip --handler meuhandler --runtime meuruntime --role arn:aws:iam:meuaccountid:role/minharole

The above command is just an example. Replace the Values `funcao-teste, meufonte.zip, meuhandler, meuruntime, meuaccountid e minharole` by the values specific to your scenario. The full documentation is found [hereto](https://docs.aws.amazon.com/pt_br/lambda/latest/dg/gettingstarted-awscli.html).

#### Considerations

The AWS CLI commands listed above are just examples. You can automate part of this import and export via shell script or python, for example, to minimize possible configuration problems by doing them manually through the Console. Develop the safest strategy for your scenario and evolve your architecture wherever possible and based on [Pillars for Resilient Workloads] ({{< ref "intro/disaster-recovery/architecture-pillars" >}} "Pillars for Resilient Workloads").

{{% notice info%}}
Consider minimally encapsulating your source code using [SAM (Serverless Application Model)](https://aws.amazon.com/serverless/sam/). This way, it is possible to transport the source code, its settings and dependencies in a single package. This dramatically simplifies the complexity of moving your installation packages between regions and follows the [Infrastructure-as-Code] ({{< ref "intro/disaster-recovery/architecture-pillars" >}} "Infrastructure-as-Code") pillar described in this material.
{{% /notice%}}

In addition to the SAM described above, you can also use tools such as AWS [CloudFormation](https://aws.amazon.com/cloudformation/) or the [AWS CDK](https://aws.amazon.com/cdk/) via command line. While executed manually, these tools minimally guarantee a *diff* of your current infrastructure, making it easier *deployment* of the evolutions of its architecture in multiple regions.

---

**References**

- [AWS CloudFormation](https://aws.amazon.com/pt/cloudformation/)
- [Add a cross-region action in CodePipeline](https://docs.aws.amazon.com/pt_br/codepipeline/latest/userguide/actions-create-cross-region.html)
- [Deploy code in multiple AWS Regions using AWS CodePipeline, AWS CodeCommit, and AWS CodeBuild](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/deploy-code-in-multiple-aws-regions-using-aws-codepipeline-aws-codecommit-and-aws-codebuild.html)
- [Pillars for Resilient Workloads]({{< ref "intro/disaster-recovery/architecture-pillars" >}} "Pillars for Resilient Workloads")
- [AWS CLI Docs - Lambda](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-awscli.html)
