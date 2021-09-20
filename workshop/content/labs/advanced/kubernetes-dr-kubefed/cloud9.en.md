---
title: "EKS - DR Environment Using Kubefed"
menutitle: "EKS - DR Environment Using Kubefed"
weight: 1
pre: "<b></b>"
hidden: true
---

{{% notice info%}}
If you haven't already created your environment on AWS Cloud9, start by creating your workspace. [Click here]({{% relref workspace %}}).
{{% /notice%}}

{{% notice note%}}
For this exercise, use **N. Virginia (us-east-1)** as the main region and the **N. California (us-west-1)** as the secondary region.
{{% /notice%}}

#### 1. Create Amazon EKS Cluster

1. Install kubectl, eksctl and helm

    ```bash
    # Install JSON Processor
    sudo yum install jq openssl -y
    export AWSACCOUNT=$(aws sts get-caller-identity | jq -r '.Account')
    export ROLE_ARN=$(aws sts get-caller-identity | jq -r '.Arn')

    # Install kubectl
    sudo curl --silent --location -o /usr/local/bin/kubectl \
      https://amazon-eks.s3.us-west-2.amazonaws.com/1.21.2/2021-07-05/bin/linux/amd64/kubectl
    sudo chmod +x /usr/local/bin/kubectl

    # Bash completion for kubectl
    kubectl completion bash >>  ~/.bash_completion
    . /etc/profile.d/bash_completion.sh
    . ~/.bash_completion

    # Install eksctl
    curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
    sudo mv -v /tmp/eksctl /usr/local/bin

    # Bash completion for eksctl
    eksctl completion bash >> ~/.bash_completion
    . /etc/profile.d/bash_completion.sh
    . ~/.bash_completion

    # Install helm (latest installer version)
    curl -sSL https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
    ```

2. Create Two Amazon EKS Clusters in the target AWS Regions
  .
  {{% notice info %}}
  *Note that it takes approximately 10-15 minutes to create a cluster in Amazon EKS, to speed up you can open a new terminal in Cloud9 IDE and create both in parallel.*
  {{% /notice %}}

    ```bash
    # Create primary cluster
    eksctl create cluster --name kubefed-cluster \
      --version 1.18 \
      --managed \
      --alb-ingress-access \
      --zones=us-east-1a,us-east-1c \
      --region=us-east-1
    ```

    *In order to speed up, create second cluster using another terminatl window.*
    ```bash  
    # Create secondary cluster
    eksctl create cluster --name kubefed-cluster \
      --version 1.18 \
      --managed \
      --alb-ingress-access \
      --zones=us-west-1a,us-west-1c \
      --region=us-west-1

    ```

    {{% notice warning %}}
  Don't stop the process (CTRL+C or Command+C), because after the cluster creation your environment will be set up. This step is important to have *kubectl* authenticated in your clusters.
  {{% /notice %}}

3. Rename kubectl config contexts to make it easier to use later

    ```bash
    aws eks update-kubeconfig --name kubefed-cluster --region us-east-1 
    aws eks update-kubeconfig --name kubefed-cluster --region us-west-1
    kubectl config rename-context $(kubectl config get-contexts --no-headers=true -o name | grep us-east-1:$AWSACCOUNT) kubefed-cluster.us-east-1
    kubectl config rename-context $(kubectl config get-contexts --no-headers=true -o name | grep us-west-1:$AWSACCOUNT) kubefed-cluster.us-west-1
    kubectl config get-contexts
    ```

#### 2. Set Up KubeFed

1. Add Helm Repository and Update

    ```bash
    # Change context to cluster in N. Virginia (us-east-1)
    kubectl config use-context kubefed-cluster.us-east-1

    # Add Helm repository and update
    helm repo add kubefed-charts https://raw.githubusercontent.com/kubernetes-sigs/kubefed/master/charts
    helm repo update
    ```

2. Install KubeFed

    ```bash
    helm --namespace kube-federation-system upgrade -i kubefed kubefed-charts/kubefed --version=0.7.0 --create-namespace
    ```

3. Confirm that controllers and webhooks are running

    ```bash
    kubectl get pods -n kube-federation-system
    ```

    The Return will look like the following

    {{< output >}}
    NAME READY STATUS RESTARTS AGE
    kubefed-admission-webhook-5bdbdc8d76-4ldxc 1/1 Running 0 25 s
    kubefed-controller-manager-7c79bbdbc6-nw8zr 1/1 Running 0 13s
    kubefed-controller-manager-7c79bbdbc6-v45xm 1/1 Running 0 15 s
    {{< /output >}}

4. Install kubefedctl and join both clusters to kubefed

    ```bash
    # Install kubefedctl
    curl -LO https://github.com/kubernetes-sigs/kubefed/releases/download/v0.7.0/kubefedctl-0.7.0-linux-amd64.tgz
    tar -zxvf kubefedctl-*.tgz
    chmod u+x kubefedctl
    sudo mv kubefedctl /usr/local/bin/ # make sure the location is in the PATH

    # Join clusters
    # Federating the cluster itself
    kubefedctl join kubefed-cluster.us-east-1 --cluster-context kubefed-cluster.us-east-1 --host-cluster-context kubefed-cluster.us-east-1 --v=2 

    # Join secondary cluster
    kubefedctl join kubefed-cluster.us-west-1 --cluster-context kubefed-cluster.us-west-1 --host-cluster-context kubefed-cluster.us-east-1 --v=2 

    # Status list
    kubectl -n kube-federation-system get kubefedclusters
    ```

    {{< output >}}
    NAME AGE READY
    kubefed-cluster.us-east-1 2m36s True
    kubefed-cluster.us-west-1 69s True
    {{< /output >}}

Wait until the last command shows that both clusters are `Ready = True` as can be seen above.

#### 3. Deploy an application in both regions

1. Create a Namespace `kubefed-minilab` and federate with both clusters

    ```bash
    # Create namespace kubefed-minilab
    kubectl create namespace kubefed-minilab

    # Federate the namespace (by default, it will be federated for all clusters in kubefed)
    kubefedctl federate namespace kubefed-minilab

    # Check if namespace is federated to both clusters
    kubectl get federatednamespace kubefed-minilab -n kubefed-minilab -o=json | jq .status.clusters
    ```

    {{< output >}}
    [{
    “name”: “kubefed-cluster.us-west-1"
    },
    {
    “name”: “kubefed-cluster.us-east-1"
    }]
    {{< /output >}}

2. Create a `FederatedDeployment` and `FederatedService` from a `Deployment` and `Service` ordinary

    ```bash
    # Create manifest for Deployment and Service of one appication
    cat << EOF > deployment.yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: example-app
      namespace: kubefed-minilab
    spec:
      selector:
        matchLabels:
          app: example-app
      replicas: 1
      template:
        metadata:
          labels:
            app: example-app
        spec:
          containers:
          - image: public.ecr.aws/i9m4f0j6/python-example-app-dr:latest
            name: example-app
            ports:
              - containerPort: 5000
            livenessProbe:
              httpGet:
                path: /health
                port: 5000
              initialDelaySeconds: 5
              periodSeconds: 5
              timeoutSeconds: 10
              failureThreshold: 5
            readinessProbe:
              httpGet:
                path: /health
                port: 5000
              initialDelaySeconds: 5
    EOF

    cat << EOF > service.yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: example-app-svc-lb
      namespace: kubefed-minilab
    spec:
      ports:
      - name: http
        port: 80
        protocol: TCP
        targetPort: 5000
      selector:
        app: example-app
      type: LoadBalancer  
    EOF

    # Criar o FederatedDeployment e FederatedService
    kubefedctl federate -f deployment.yaml > federated_deployment.yaml
    kubefedctl federate -f service.yaml > federated_service.yaml
    ```

3. Deploy `FederatedDeployment` and `FederatedService`

    ```bash
    kubectl apply -f federated_deployment.yaml
    kubectl apply -f federated_service.yaml
    ```

4. List the resources of the kubefed-minilab namespace across all clusters (Wait a few minutes until the container is in state `Running`)

    ```bash
    for c in `kubectl config get-contexts --no-headers=true -o name`
    do
      printf "\n------------\n\nCluster Resources: [$c]\n"
      kubectl get all -n kubefed-minilab --context=$c
    done
    ```

#### 4. Access the application in both regions

1. Access the application of each region through your browser

    ```bash
    # List the load balancer addresses
    for c in `kubectl config get-contexts --no-headers=true -o name`
    do
      printf "Cluster Load Balancer [$c]: "
      echo http://$(kubectl get service example-app-svc-lb -n kubefed-minilab --context=$c -o json | jq -r '.status.loadBalancer.ingress[0].hostname')
    done
    ```

Simply access the load balancer address for each of the regions through your browser. The container used in the deployment returns `instance_metadata` of the instance on which the container is running. In this metadata it is possible among other information to see which region it is running. For more information on `instance_metadata` access [is documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html).

#### 5. Configure Route53 Failover Between the Two Regions

In this step of the exercise we will configure AWS Route53 with previously created DNS entries from our two LoadBalancers in different regions.

1. To create a private zone you will need to associate it with a VPC, so let's get the ID from our VPC where we climbed our Cloud9 EC2.

    ```bash
    # Get Default VPC ID
    export VPC_ID=$(aws ec2 describe-vpcs --filters Name=isDefault,Values=true | jq -r '.Vpcs[0].VpcId')

    # Create private hosted zone and associate to Default VPC
    export ZONE_ID=$(aws route53 create-hosted-zone --name my.private.hz --vpc "VPCRegion=us-east-1,VPCId=${VPC_ID}" --hosted-zone-config 'Comment=private,PrivateZone=true' --caller-reference $(date "+%Y%m%d%H%M%S") | jq .HostedZone.Id | cut -d '/' -f3)
    ```

Now that we've created our DNS Zone we'll configure active-passive failover. For more information about routing policies on AWS, visit [this documentation](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html).

2. Let's create the health check for our primary endpoint that will be in **us-east-1**

    ```bash
    # Export Load Balancer DNS
    export LB_US_EAST_1=$(kubectl get svc example-app-svc-lb -nkubefed-minilab --context kubefed-cluster.us-east-1 -o json | jq '.status.loadBalancer.ingress[0].hostname')
    export LB_US_WEST_1=$(kubectl get svc example-app-svc-lb -nkubefed-minilab --context kubefed-cluster.us-west-1 -o json | jq '.status.loadBalancer.ingress[0].hostname')
    ```

    ```bash
    # Health check policy
    cat > create-health-check.json << EOF
    {
      "Type": "HTTP",
      "ResourcePath": "/health",
      "FullyQualifiedDomainName": ${LB_US_EAST_1},
      "RequestInterval": 30,
      "FailureThreshold": 3
    } 
    EOF
    ```

    ```bash
    # Create Healthcheck for Route53
    export HEALTH_ID=$(aws route53 create-health-check --caller-reference $(date "+%Y%m%d%H%M%S") --health-check-config file://create-health-check.json | jq .HealthCheck.Id)
    ```

3. Creating our primary endpoint using Failover routing policy.

    ```bash
    # Create DNS entry definition - Primary
    cat > failover.json << EOF
    {
        "Comment": "Failover DNS Primary",
        "Changes": [
          {
            "Action": "CREATE",
            "ResourceRecordSet": {
              "Name": "dr-app.my.private.hz",
              "Type": "CNAME",
              "SetIdentifier": "primary-lb",
              "Failover": "PRIMARY",
              "TTL": 60,
              "ResourceRecords": [
                {
                  "Value": ${LB_US_EAST_1}
                }
              ],
              "HealthCheckId": ${HEALTH_ID}
            }
          }
        ]
    }
    EOF
    ```

    ```bash
    # Create private hosted zone entry
    aws route53 change-resource-record-sets --hosted-zone-id $(echo $ZONE_ID | tr -d '"'
    ) --change-batch file://failover.json
    ```

    The healthcheck will be active in 30 seconds.

4. Creating our secondary endpoint using Failover routing policy.

    ```bash
    # Create DNS entry definition - Secondary
    cat > failover.json << EOF
    {
        "Comment": "Failover DNS secondary",
        "Changes": [
          {
            "Action": "CREATE",
            "ResourceRecordSet": {
              "Name": "dr-app.my.private.hz",
              "Type": "CNAME",
              "SetIdentifier": "secondary-lb",
              "Failover": "SECONDARY",
              "TTL": 60,
              "ResourceRecords": [
                {
                  "Value": ${LB_US_WEST_1}
                }
              ]
            }
          }
        ]
    }
    EOF
    ```

    ```bash
    # Create private hosted zone entry
    aws route53 change-resource-record-sets --hosted-zone-id $(echo $ZONE_ID | tr -d '"'
    ) --change-batch file://failover.json
    ```

Note that we're associating healthcheck with the primary only.

5. Resolve the DNS we created and verify which LoadBalancer endpoint it is pointing to (Because we associate the hosted zone with the VPC of the Cloud9 instance we were able to resolve DNS).

    ```bash
    curl -XGET http://dr-app.my.private.hz
    ```

If everything is right, the result of the region should be.

    {{< output >}}
    {
    “region”: “us-east-1"
    }
    {{< /output >}}

#### 6. Simulate us-east-1 primary region failure

1. Let's update our `FederatedDeployment` making our application's POD only exist in the region `us-west-1`.

    ```bash
    cat > federated_deployment.yaml << EOF
    apiVersion: types.kubefed.io/v1beta1
    kind: FederatedDeployment
    metadata:
      name: example-app
      namespace: kubefed-minilab
    spec:
      placement:
        clusters:
          - name: kubefed-cluster.us-west-1
      template:
        spec:
          replicas: 1
          selector:
            matchLabels:
              app: example-app
          template:
            metadata:
              labels:
                app: example-app
            spec:
              containers:
              - image: public.ecr.aws/i9m4f0j6/python-example-app-dr:latest
                livenessProbe:
                  failureThreshold: 5
                  httpGet:
                    path: /health
                    port: 5000
                  initialDelaySeconds: 5
                  periodSeconds: 5
                  timeoutSeconds: 10
                name: example-app
                ports:
                - containerPort: 5000
                readinessProbe:
                  httpGet:
                    path: /health
                    port: 5000
                  initialDelaySeconds: 5
    EOF
    ```

2. Let's apply our new manifesto

    ```bash
    kubectl apply -f federated_deployment.yaml
    ```

    Wait a few minutes until the propagate update in our DNS zone.

3. Run the CURL again on our endpoint.

    ```bash
    curl -XGET http://dr-app.my.private.hz
    ```

If everything is right, the result of the region should be.

    ```json
    {
      "region": "us-west-1"
    }
    ```

#### 7. Cleaning up

1. Delete `FederatedDeployment` and `FederatedService` (It will be deleted from both clusters).

    ```bash
    kubectl delete -f federated_deployment.yaml
    kubectl delete -f federated_service.yaml
    ```

2. Delete the cluster and its associated nodes with the following command.

    ```bash
    eksctl delete cluster --region=us-east-1 --name kubefed-cluster
    eksctl delete cluster --region=us-west-1 --name kubefed-cluster
    ```
