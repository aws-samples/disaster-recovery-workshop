---
hidden: true
type: lab
---

#### Create an AWS Lambda Function

*If you already have a Lambda function created, you can skip to the next step at **Export the Lambda function***

1.  Go to the AWS CloudShell console: <https://console.aws.amazon.com/cloudshell/home>. <br>For this example, choose the region **us-east-1 (Northern Virginia)**. CloudShell is not yet available in all regions.

2.  With the command prompt ready, we need **create the role** to allow execution access to the Lambda function:

    ```bash
    aws iam create-role --role-name lambda-execution --assume-role-policy-document '{"Version": "2012-10-17","Statement": [{ "Effect": "Allow", "Principal": {"Service": "lambda.amazonaws.com"}, "Action": "sts:AssumeRole"}]}'
    ```

    Permission is now required before the function can be executed. To do this, add the policy *AWS Lambda Basic Executive Role* to the role created above:

    ```bash
    aws iam attach-role-policy --role-name lambda-execution --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
    ```

3.  We will now create the lambda function via CLI. To do this, run the command below:
    ```bash
    echo "exports.handler = async (event) => {
        console.log('chamando função lambda...', event);
        const response = {
            'statusCode': 200,
            'body': JSON.stringify('Testando o import e export de funções Lambda!'),
        };
        return response;
    };" > index.js
    ```

4.  The above command created the file **index.js** containing the source code for our Lambda function. Let's now package the function into a.zip file so we can register it to the AWS Lambda service for the region in question:
    ```bash
    zip multi-region-test-function index.js
    ```

5.  We can now register the function through the command below:

    ```bash
    export ACC_ID=$(aws sts get-caller-identity --query Account --output text)
    aws lambda create-function --function-name multi-region-test-function --zip-file fileb://multi-region-test-function.zip --handler index.handler --runtime nodejs14.x --role arn:aws:iam::$ACC_ID:role/lambda-execution
    ```

    *   In **Function name (function name)** insert **multi-region-test-function**. In Runtime, choose **Node.js 12x**.
    *   To create the role, select **Create (Create)**. The function will be created and you will be redirected to the console's function editor.
    *   Copy and paste the code below:

    ```javascript
    exports.handler = async (event) => {
       console.log("chamando função lambda...")
       const response = {
            statusCode: 200,
            body: JSON.stringify('Testando o import e export de funções Lambda!'),
       };
       return response;
    };
    ```

6.  *(Optional)* If the function already exists in the AWS Lambda service, you can update the function source code using the command below:
    ```bash
    aws lambda update-function-code --function-name multi-region-test-function --zip-file fileb://multi-region-test-function.zip
    ```

7.  Let's now test the created function. Use the command below to invoke the Lambda function via the command line:
    ```bash
    aws lambda invoke --function-name multi-region-test-function --payload $(echo '{ "key_sample": "value_sample" }' | base64) --log-type Tail --query 'LogResult' --output text out | base64 -d
    ```
    We have an INFO line containing the console message inside the function along with the payload sent, which indicates that the function was executed successfully.

8.  Ready! Your Lambda function was successfully created via the AWS CLI with CloudShell.

#### Export the created Lambda function

1.  Go to the AWS CloudShell console: <https://console.aws.amazon.com/cloudshell/home>. <br>For this example, choose the region **us-east-1 (Northern Virginia)**.
2.  *(Optional)* With the command prompt ready, we will **list functions** Lambda existing in each region. To do this, use the command below, replacing the region if you want:
    ```bash
    aws lambda list-functions --region sa-east-1
    ```
3.  In this lab, we already know the name of the function we created earlier. In this way, we will export it and generate a.zip with the source code through the command:
    ```bash
    aws lambda get-function --function-name multi-region-test-function --query 'Code.Location' | xargs wget -O multi-region-test-function-exp.zip
    ```
    *You can extract other metadata from the Lambda function, such as timeout settings or environment variables, for example. For this lab, we focused only on the source code for the simplicity of the lab*
4.  Ready. We already have the source code for our function and we can import it into another region.

#### Import the Lambda function into the second region

*We will update an existing role in another region. In this example, we will use the region **sa-east-1 (North America - Sao Paulo)**. If the role does not already exist in this region, perform the steps in the step **Create a Lambda function** described at the beginning of this lab.*

1.  Go to the AWS CloudShell console: <https://console.aws.amazon.com/cloudshell/home>. <br>For this example, choose the region **us-east-1 (Northern Virginia)**.
2.  We will update the function **multi-region-test-function** in the region **sa-east-1 (South America - Sao Paulo)**. To do this, run the following command:
    ```bash
    aws lambda update-function-code --function-name multi-region-test-function --region sa-east-1 --zip-file fileb://multi-region-test-function-exp.zip
    ```
3.  Let's now test the import by calling the function. For this:
    ```bash
    aws lambda invoke --function-name multi-region-test-function --payload $(echo '{ "key_sample": "value_sample" }' | base64) --log-type Tail --query 'LogResult' --region sa-east-1 --output text out | base64 -d
    ```
    <br/>We can validate that the function was updated because the INFO log line now also writes the received payload, which did not happen with the function created at the beginning of this lab.
4.  Ready! Its Lambda function was created in another region.

#### Deleting resources

1.  Delete the lambda functions created in both regions with the following commands:
    ```bash
    aws lambda delete-function --function-name multi-region-test-function --region us-east-1
    aws lambda delete-function --function-name multi-region-test-function --region sa-east-1
    ```
