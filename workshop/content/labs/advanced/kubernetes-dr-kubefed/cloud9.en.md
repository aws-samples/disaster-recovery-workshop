---
title: "EKS - DR Environment Using Kubefed"
menutitle: "EKS - DR Environment Using Kubefed"
weight: 1
pre: "<b></b>"
hidden: true
---

#### 1. Create Amazon EKS Cluster

1.1 Install kubectl, eksctl and helm

```bash
# Instalar kubectl
sudo curl --silent --location -o /usr/local/bin/kubectl \
   https://amazon-eks.s3.us-west-2.amazonaws.com/1.17.11/2020-09-18/bin/linux/amd64/kubectl
sudo chmod +x /usr/local/bin/kubectl

# Configurar bash completion para o kubectl
kubectl completion bash >>  ~/.bash_completion
. /etc/profile.d/bash_completion.sh
. ~/.bash_completion

# Instalar eksctl
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv -v /tmp/eksctl /usr/local/bin

# Configurar bash completion para o eksctl
eksctl completion bash >> ~/.bash_completion
. /etc/profile.d/bash_completion.sh
. ~/.bash_completion

# Instalar helm
curl -sSL https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
```

1.2 Create Two Amazon EKS Clusters in Target Regions

```bash
eksctl create cluster --region=us-east-1 --name kubefed-cluster --version 1.18 --managed --alb-ingress-access --zones=us-east-1a,us-east-1b,us-east-1c 
eksctl create cluster --region=us-east-2 --name kubefed-cluster --version 1.18 --managed --alb-ingress-access --zones=us-east-2a,us-east-2b,us-east-2c 
```

{{% notice info%}}
Note that it takes approximately 9 minutes to create a cluster in Amazon EKS, to speed up you can open a new terminal in Cloud9 and create both in parallel.
{{% /notice%}}

1.3 Rename kubectl config contexts to make it easier to use later

```bash
kubectl config rename-context $(kubectl config get-contexts --no-headers=true -o name | grep us-east-1) kubefed-cluster.us-east-1
kubectl config rename-context $(kubectl config get-contexts --no-headers=true -o name | grep us-east-2) kubefed-cluster.us-east-2
kubectl config get-contexts
```

#### 2. Set Up KubeFed

2.1 Add Helm Repository and Update

```bash
# Mudar o contexto para o cluster na região N. Virginia (us-east-1)
kubectl config use-context kubefed-cluster.us-east-1
# Adicionar repositório Helm e atualizar
helm repo add kubefed-charts https://raw.githubusercontent.com/kubernetes-sigs/kubefed/master/charts
helm repo update
```

2.2 Install KubeFed

```bash
helm --namespace kube-federation-system upgrade -i kubefed kubefed-charts/kubefed --version=0.7.0 --create-namespace
```

2.3 Confirm that controllers and webhooks are running

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

2.4 Install kubefedctl and join both clusters to kubefed

```bash
# Instalar o kubefedctl
curl -LO https://github.com/kubernetes-sigs/kubefed/releases/download/v0.7.0/kubefedctl-0.7.0-linux-amd64.tgz
tar -zxvf kubefedctl-*.tgz
chmod u+x kubefedctl
sudo mv kubefedctl /usr/local/bin/ # make sure the location is in the PATH

# Juntar os clusters
kubefedctl join kubefed-cluster.us-east-1 --cluster-context kubefed-cluster.us-east-1 --host-cluster-context kubefed-cluster.us-east-1 --v=2 # Federating the cluster Itself
kubefedctl join kubefed-cluster.us-east-2 --cluster-context kubefed-cluster.us-east-2 --host-cluster-context kubefed-cluster.us-east-1 --v=2 

# Listar status
kubectl -n kube-federation-system get kubefedclusters
```

{{< output >}}
NAME AGE READY
kubefed-cluster.us-east-1 2m36s True
kubefed-cluster.us-east-2 69s True
{{< /output >}}

Wait until the last command shows that both clusters are `Ready = True` as can be seen above.

#### 3. Deploy an application in both regions

3.1 Create a Namespace `kubefed-minilab` and federate with both clusters

```bash
# Criar namespace kubefed-minilab
kubectl create namespace kubefed-minilab

# Federar a namespace (por padrão será federada para todos os clusters do kubefed)
kubefedctl federate namespace kubefed-minilab

# Confirmar que a namespace está federada nos dois clusters
kubectl get federatednamespace kubefed-minilab -n kubefed-minilab -o=json | jq .status.clusters
```

{{< output >}}
\[
{
“name”: “kubefed-cluster.us-east-2"
},
{
“name”: “kubefed-cluster.us-east-1"
}
]
{{< /output >}}

3.2 Create a `FederatedDeployment` and `FederatedService` from a `Deployment` and `Service` ordinary

```bash

# Criar manifestos de Deployment e Service de uma aplicação
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

3.3 Deploy `FederatedDeployment` and `FederatedService`

```bash
kubectl apply -f federated_deployment.yaml
kubectl apply -f federated_service.yaml
```

3.4 List the resources of the kubefed-minilab namespace across all clusters (Wait a few minutes until the container is in state `Running`)

```bash
for c in `kubectl config get-contexts --no-headers=true -o name`
do
  printf "\n------------\n\nRecursos do cluster: [$c]\n"
  kubectl get all -n kubefed-minilab --context=$c
done
```

#### 4. Access the application in both regions

4.1 Access the application of each region through your browser

```bash
# Listar o endereço do balanceador de carga de cada uma das regiões
for c in `kubectl config get-contexts --no-headers=true -o name`
do
  printf "Load balancer do cluster [$c]: "
  echo http://$(kubectl get service example-app-svc-lb -n kubefed-minilab --context=$c -o json | jq -r '.status.loadBalancer.ingress[0].hostname')
done
```

Simply access the load balancer address for each of the regions through your browser. The container used in the deployment returns `instance_metadata` of the instance on which the container is running. In this metadata it is possible among other information to see which region it is running. For more information on `instance_metadata` access [is documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html).

#### 5. Configure Route53 Failover Between the Two Regions

In this step of the exercise we will configure AWS Route53 with previously created DNS entries from our two LoadBalancers in different regions.

5.1 To create a private zone you will need to associate it with a VPC, so let's get the ID from our VPC where we climbed our Cloud9 EC2.

```bash
export VPC_ID=$(aws ec2 describe-instances --filters 'Name=tag:Name,Values=*workshop*' --region us-east-1 | jq '.Reservations[0].Instances[0].VpcId')

# Criar hosted zona na AWS associando a VPC da instancia do Cloud9

export ZONE_ID=$(aws route53 create-hosted-zone --name my.private.hz --vpc "VPCRegion=us-east-1,VPCId=${VPC_ID}" --hosted-zone-config 'Comment=private,PrivateZone=true' --caller-reference $(date "+%Y%m%d%H%M%S") | jq .HostedZone.Id | cut -d '/' -f3)
```

Now that we've created our DNS Zone we'll configure active-passive failover. For more information about routing policies on AWS, visit [this documentation](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html).

5.2 Let's create the health check for our primary endpoint that will be in **us-east-1**

```bash
# Exportar DNS do LoadBalancer para uma variavel de ambiente

export LB_US_EAST_1=$(kubectl get svc example-app-svc-lb -nkubefed-minilab --context kubefed-cluster.us-east-1 -o json | jq '.status.loadBalancer.ingress[0].hostname')

export LB_US_EAST_2=$(kubectl get svc example-app-svc-lb -nkubefed-minilab --context kubefed-cluster.us-east-2 -o json | jq '.status.loadBalancer.ingress[0].hostname')
```

```bash
# Criar arquivo de healthcheck

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
# Criar Healthcheck no Route53

export HEALTH_ID=$(aws route53 create-health-check --caller-reference $(date "+%Y%m%d%H%M%S") --health-check-config file://create-health-check.json | jq .HealthCheck.Id)
```

5.3 Creating our primary endpoint using Failover routing policy.

```bash
# Criando arquivo para criar entrada de DNS

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
# Criando entrada na zona de DNS

aws route53 change-resource-record-sets --hosted-zone-id $(echo $ZONE_ID | tr -d '"'
) --change-batch file://failover.json
```

Wait a few minutes for healthcheck to be active.

5.4 Creating our secondary endpoint using Failover routing policy.

```bash

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
              "Value": ${LB_US_EAST_2}
            }
          ]
        }
      }
    ]
}
EOF
```

```bash
# Criando entrada na zona de DNS

aws route53 change-resource-record-sets --hosted-zone-id $(echo $ZONE_ID | tr -d '"'
) --change-batch file://failover.json
```

Note that we're associating healthcheck with the primary only.

5.5 Resolve the DNS we created and verify which LoadBalancer endpoint it is pointing to (Because we associate the hosted zone with the VPC of the Cloud9 instance we were able to resolve DNS).

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

6.1 Let's update our `FederatedDeployment` making our application's POD only exist in the region `us-east-2`.

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
      - name: kubefed-cluster.us-east-2
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

6.2 Let's apply our new manifesto

```bash
kubectl apply -f federated_deployment.yaml
```

Wait a few minutes until the propagate update in our DNS zone.

6.3 Run the CURL again on our endpoint.

```bash
curl -XGET http://dr-app.my.private.hz
```

If everything is right, the result of the region should be.

```json
{
  "region": "us-east-2"
}
```

#### 7. Cleaning up

7.1 Delete `FederatedDeployment` and `FederatedService` (It will be deleted from both clusters).

```bash
kubectl delete -f federated_deployment.yaml
kubectl delete -f federated_service.yaml
```

7.2 Delete the cluster and its associated nodes with the following command.

```bash
eksctl delete cluster --region=us-east-1 --name kubefed-cluster
eksctl delete cluster --region=us-east-2 --name kubefed-cluster
```
