---
title: "Amazon ECR - Image Registry Replication"
menutitle: "ECR - Image Registry Replication"
weight: 1
pre: "<b></b>"
hidden: true
---


{{% notice info%}}
If you haven't already created your environment on AWS Cloud9, start by creating your workspace. [Click here](pt/../../../../prereqs/workspace/workspace).
{{% /notice%}}

{{% notice note%}}
For this exercise, use **Virginia (us-east-1)** like the main region and the **California (us-west-1)** like the secondary region.
{{% /notice%}}

#### Create a Repository in the Registry

1.  Save your AWS account ID to an environment variable.
    ```bash
    sudo yum install jq -y
    export AWSACCOUNT=$(aws sts get-caller-identity | jq -r '.Account')
    ```

2.  Create a repository in *registry* private. Note: Every AWS Account already has one *registry* private.

    ```bash
    aws ecr create-repository \
        --repository-name ecr-repository --region us-east-1
    ```

#### Create an image and push to the created Repository

1.  Create index.html

    ```bash

    cat > index.html << EOF
        <!doctype html>
        <html lang="en">
            <head>
                <meta charset="utf-8">
                <title>Docker Nginx</title>
            </head>
            <body>
                <h2>Hello from Nginx container</h2>
            </body>
        </html>
    EOF

    ```

2.  Create the Dockerfile

    ```bash
    cat > Dockerfile << EOF
    FROM nginx:latest
    COPY ./index.html /usr/share/nginx/html/index.html
    EOF
    ```

3.  Create the docker image with the repository name (replace the account id according to your environment)

    ```bash

    docker build -t $AWSACCOUNT.dkr.ecr.us-east-1.amazonaws.com/ecr-repository:webapp .

    ```

4.  To push the image to the repository using docker, you must authenticate to the repository first. To do this, use the get-login command and get the password to authenticate the docker with the docker login command. For the command below, replace the account id according to your environment.

    ```bash

    aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin $AWSACCOUNT.dkr.ecr.us-east-1.amazonaws.com

    ```

5.  After authenticating, simply push using the docker push command and replace the account id according to your environment.

    ```bash

    docker push $AWSACCOUNT.dkr.ecr.us-east-1.amazonaws.com/ecr-repository:webapp
        
    ```

#### Set up replication for another region in the image log

1.  Get the record id.

    ```bash

    export REGISTRYID=$(aws ecr describe-registry | jq -r '.registryId')

    ```

2.  Create a json file with configuration parameters. Replace the registration id for your environment. Notice that the replication configuration will replicate to the **California (us-west-1)**.

    ```bash

    cat > replication.json << EOF
    { 
        "rules": [ 
            { 
                "destinations": [ 
                    {
                        "region": "us-west-1", 
                        "registryId": "$REGISTRYID" 
                    } 
                ] 
            } 
        ] 
    } 
    EOF

    ```

3.  Enable replication with the command below.

    ```bash
    aws ecr put-replication-configuration \
    --replication-configuration file://replication.json --region us-east-1
    ```

#### Make an update to the image

1.  Update the container image by generating a new version with the following command. Replace the account id according to your environment.

    ```bash

    docker build -t $AWSACCOUNT.dkr.ecr.us-east-1.amazonaws.com/ecr-repository:webapp2 .

    ```

2.  Push the new version. Notice that the push takes place to the region of origin (in this case N.Viginia (us-east-1)).

    ```bash

    docker push $AWSACCOUNT.dkr.ecr.us-east-1.amazonaws.com/ecr-repository:webapp2

    ```

#### Validate that the image is in both regions

1.  List the image repositories from both regions. On the return of the command, note the parameter *RepositoryURI*.

    ```bash

    aws ecr describe-repositories --region us-east-1
    aws ecr describe-repositories --region us-west-1

    ```

2.  List images from both regions. Note that the images are the same size, the same manifest. However, the push date in the repository is slightly different (ImagePushedAt parameter).

    ```bash
    aws ecr describe-images --repository-name ecr-repository --region us-east-1
    aws ecr describe-images --repository-name ecr-repository --region us-west-1
    ```

#### Cleaning up

1.  Delete images created in repositories
    ```bash
    aws ecr batch-delete-image \
    --repository-name ecr-repository \
    --image-ids imageTag=webapp imageTag=webapp2 --region us-east-1
    aws ecr batch-delete-image \
    --repository-name ecr-repository \
    --image-ids imageTag=webapp imageTag=webapp2 --region us-west-1
    ```

2.  Delete created repositories
    ```bash
    aws ecr delete-repository --repository-name ecr-repository --region us-east-1
    aws ecr delete-repository --repository-name ecr-repository --region us-west-1
    ```
