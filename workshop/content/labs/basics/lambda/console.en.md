---
hidden: true
---

#### Create an AWS Lambda Function

1.  Go to the AWS Lambda console: <https://console.aws.amazon.com/lambda>. <br>For this example, choose the region **us-east-1 (Northern Virginia)**.
2.  In the navigation pane, on the left side of the console, select **Functions (Functions)**.
3.  Choose **Create Function (Create Role)**.
    ![create function](/images/lambda-create-function.png)

    *   Select **Author from scratch (create from scratch)**
    *   In **Function name (function name)** insert **multi-region-test-function**. In Runtime, choose **Node.js 14.x**.
    *   To create the role, select **Create (Create)**.
        ![create function](/images/lambda-create-function-form.png)
    *   The function will be created and you will be redirected to the console's function editor. In **Function Code (Function Code)** Copy and paste the code below:

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

    *   Click **Test (test)** in the top right corner. A pop-up will appear prompting you to set up a test event.
        ![create function](/images/lambda-test-button.png)
    *   Select **Create new test event (Create new test event)**, give it the name **eventdata** in the field **Event name (event name)** and click **Create (Create)**
    *   Click again **Test**. The message **Execution result: succeeded** should appear. By clicking on details, a *payload* JSON should appear with the text \* Hello from Lambda! \*.
        ![create function](/images/lambda-success.png)
4.  Your Lambda function was created successfully! We will now export it from the current region for further import into another region.

#### Export the created Lambda function

1.  Go to the AWS Lambda console: <https://console.aws.amazon.com/lambda>. <br>Remembering that for this example we are using the region for now **us-east-1 (Northern Virginia)**.
2.  In the list of roles, select the role **multi-region-test-function** created earlier. The Configuration tab will appear.
3.  Click Actions -> Export Function
    ![create function](/images/lambda-export.png)
    *   A pop-up will open offering two options for download: Download AWS SAM file or Download deployment package.
    *   Select Deployment package and wait for the download. Note that the downloaded file is a zip file, but with no extension. After downloading, rename the file with the .zip extension so we can use it later.

#### Import the Lambda function into the second region

1.  Go to the AWS Lambda console: <https://console.aws.amazon.com/lambda>.
2.  Select the region in the combo in the upper right corner of the console. <br/>For this example, I will use the region **sa-east-1 - South America (Sao Paulo)**
3.  If the function doesn't exist:
    *   Perform steps 1.2 and 3 in item 3 of the section **Create an AWS Lambda Function** above.
4.  In the list of roles, select the role **multi-region-test-function** created. The Configuration tab will appear.
    *   Under Function Code, click Actions, upload to .zip file.
        ![upload](/images/lambda-upload.png)
        *   You can also import your function through an S3 bucket. For this, select Upload a file from Amazon S3 and enter the URL where the.zip is located.
    *   Select the previously downloaded zip file (don't forget to rename it with the .zip extension) and click Save.
    *   (Optional) If you want to test your function, set up the test payload as we did in steps 5.6 and 7 of item 3 in **Create a Lambda function** above.
5.  Ready! Its Lambda function has been copied from one region to another.

#### Cleaning up

1.  Delete the lambda functions created in both regions.
