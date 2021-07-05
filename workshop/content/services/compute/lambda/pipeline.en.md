***

title: “AWS Lambda - CI/CD Pipeline”
MenUTitle: “CI/CD Pipeline”
weight: 5
AWS Services:

*   AWS Lambda
    tags:
*   Warm Standby

***

It consists of setting up your Code Delivery Pipeline by installing via CodePipeline and CodeDeploy AWS Lambda functions in more than one region. You can install in more than one region at the same time, or you can parameterize your Pipeline by informing you of the region to install at the time of running the Pipeline. CodePipeline supports natively [Cross-Region Stocks](https://docs.aws.amazon.com/codepipeline/latest/userguide/actions-create-cross-region.html).
{{% notice tip%}}
This is the recommended approach. This way the version of your AWS Lambda functions will be synchronized because they use the same storage source through a versioning tool.
{{% /notice%}}

This strategy brings more visibility into the state of the facilities, allowing them *deployments* in each region be traceable. Describe your AWS Lambda functions via [SAM (Serverless Application Model)](https://aws.amazon.com/serverless/sam/) to ensure that parameters such as permissions, memory, timeout, environment variables, and other options are synchronized. Este [blog post](https://aws.amazon.com/blogs/devops/using-aws-codepipeline-to-perform-multi-region-deployments/) describes how to use CodePipeline and CodeDeploy to install multi-region artifacts. [In this pattern](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/deploy-code-in-multiple-aws-regions-using-aws-codepipeline-aws-codecommit-and-aws-codebuild.html) of [Prescriptive Guidance for DevOps](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/devops-pattern-list.html), we demonstrated how to automate multi-region deployment using CloudFormation, which contains your AWS Lambda code, your required configurations, and dependencies in a unified way.

***

**References**

*   [Using AWS CodePipeline to Perform Multi-Region Deployments](https://aws.amazon.com/blogs/devops/using-aws-codepipeline-to-perform-multi-region-deployments/)
*   [Add a cross-region action in CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/actions-create-cross-region.html)
*   [Deploy code in multiple AWS Regions using AWS CodePipeline, AWS CodeCommit, and AWS CodeBuild](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/deploy-code-in-multiple-aws-regions-using-aws-codepipeline-aws-codecommit-and-aws-codebuild.html)
