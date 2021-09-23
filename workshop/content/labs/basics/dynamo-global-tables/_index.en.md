---
title: "Amazon DynamoDB - Global Tables"
menutitle: "DynamoDB - Global Tables"
chapter: false
weight: 1
pre: "<b></b>"
tags: [Active-Active]
type: Lab
awsServices:
 - Amazon DynamoDB
---

### Objective

This exercise will show you the steps for using the Global Tables engine for Amazon DynamoDB tables. If you want more information [click here]({{< ref "/services/databases/dynamodb/dynamo-global-table" >}} "Amazon DynamoDB").

**By the end of this exercise, you will be able to:**

* Replicate Amazon DynamoDB tables in other AWS Regions


**Estimated Duration:** 15 minutes

**Approximate Cost**: 1 USD

### Execution
{{< tabs name="labs_types" >}} 
{{< tab name="AWS CloudShell" include="cloudshell" />}}  
{{< /tabs >}}

### Conclusion

With this exercise, it was possible to verify how replicated Amazon DynamoDB tables can be used in a strategy of the type. *Active-Active*, both for reading and for data persist.

**Source:** [Amazon DynamoDB - Developer Guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/V2globaltables.tutorial.html)


