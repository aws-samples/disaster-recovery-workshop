---
title: "EKS - Ambiente de DR utilizando Kubefed"
menutitle: "EKS - Ambiente de DR utilizando Kubefed"
weight: 1
pre: "<b></b>"
hidden: true
---

#### 1. Criar cluster do Amazon EKS
 
1.1 Instalar o kubectl, eksctl e helm
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

1.2 Criar dois cluster do Amazon EKS em regiões destintas
```bash
eksctl create cluster --region=us-east-1 --name kubefed-cluster --version 1.18 --managed --alb-ingress-access --zones=us-east-1a,us-east-1b,us-east-1c 
eksctl create cluster --region=us-east-2 --name kubefed-cluster --version 1.18 --managed --alb-ingress-access --zones=us-east-2a,us-east-2b,us-east-2c 
```
{{% notice info %}}
Note que demora aproximadamente 9 minutos para criar um cluster no Amazon EKS, para agilizar você pode abrir um novo terminal no Cloud9 e criar os dois em paralelo. 
{{% /notice %}}

1.3 Renomear os contextos do kubectl config para facilitar a utilização posteriormente
```bash
kubectl config rename-context $(kubectl config get-contexts --no-headers=true -o name | grep us-east-1) kubefed-cluster.us-east-1
kubectl config rename-context $(kubectl config get-contexts --no-headers=true -o name | grep us-east-2) kubefed-cluster.us-east-2
kubectl config get-contexts
```


#### 2. Configurar o KubeFed

2.1 Adicionar repositório Helm e atualizar
```bash
# Mudar o contexto para o cluster na região N. Virginia (us-east-1)
kubectl config use-context kubefed-cluster.us-east-1
# Adicionar repositório Helm e atualizar
helm repo add kubefed-charts https://raw.githubusercontent.com/kubernetes-sigs/kubefed/master/charts
helm repo update
```
2.2 Instalar o KubeFed
```bash
helm --namespace kube-federation-system upgrade -i kubefed kubefed-charts/kubefed --version=0.7.0 --create-namespace
```
2.3 Confirmar se os controllers e webhooks estão executando
```bash
kubectl get pods -n kube-federation-system
```

O Retorno será parecido com o seguinte

{{< output >}}
NAME                                          READY   STATUS    RESTARTS   AGE
kubefed-admission-webhook-5bdbdc8d76-4ldxc    1/1     Running   0          25s
kubefed-controller-manager-7c79bbdbc6-nw8zr   1/1     Running   0          13s
kubefed-controller-manager-7c79bbdbc6-v45xm   1/1     Running   0          15s
{{< /output >}}

2.4 Instalar o kubefedctl e juntar os dois cluster ao kubefed
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
NAME                        AGE     READY
kubefed-cluster.us-east-1   2m36s   True
kubefed-cluster.us-east-2   69s     True
{{< /output >}}

Aguarde até que o último comando mostre que ambos os clusters estão `Ready = True` conforme pode ser visto acima.

#### 3. Fazer deploy de uma aplicação nas duas regiões

3.1 Criar uma namespace `kubefed-minilab` e federar com os dois clusters
```bash
# Criar namespace kubefed-minilab
kubectl create namespace kubefed-minilab

# Federar a namespace (por padrão será federada para todos os clusters do kubefed)
kubefedctl federate namespace kubefed-minilab

# Confirmar que a namespace está federada nos dois clusters
kubectl get federatednamespace kubefed-minilab -n kubefed-minilab -o=json | jq .status.clusters
```

{{< output >}}
[
  {
    "name": "kubefed-cluster.us-east-2"
  },
  {
    "name": "kubefed-cluster.us-east-1"
  }
]
{{< /output >}}

3.2 Criar um `FederatedDeployment` e `FederatedService` a partir de um `Deployment` e `Service` comum
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

3.3 Fazer deploy do `FederatedDeployment` e `FederatedService`
```bash
kubectl apply -f federated_deployment.yaml
kubectl apply -f federated_service.yaml
```

3.4 Listar os recursos da namespace kubefed-minilab em todos os clusters (Aguarde alguns minutos até que o container esteja no estado `Running`)

```bash
for c in `kubectl config get-contexts --no-headers=true -o name`
do
  printf "\n------------\n\nRecursos do cluster: [$c]\n"
  kubectl get all -n kubefed-minilab --context=$c
done
```

#### 4. Acessar a aplicação nas duas regiões 

4.1 Acessar a aplicação de cada região pelo seu navegador
```bash
# Listar o endereço do balanceador de carga de cada uma das regiões
for c in `kubectl config get-contexts --no-headers=true -o name`
do
  printf "Load balancer do cluster [$c]: "
  echo http://$(kubectl get service example-app-svc-lb -n kubefed-minilab --context=$c -o json | jq -r '.status.loadBalancer.ingress[0].hostname')
done
```
Basta acessar o endereço do balanceador de carga de cada uma das regiões pelo seu navegador. O container utilizado no deployment retorna o `instance_metadata` da instancia na qual o container está executando. Nestes metadados é possível dentre outras informações ver qual a região que ele está executando. Para mais informações sobre `instance_metadata` acesse [está documentação](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/ec2-instance-metadata.html).

#### 5. Configurar Route53 failover entre as duas regiões

Nesta etapa do exercício iremos configurar o AWS Route53 com as entradas de DNS criadas anteriormente dos nossos dois LoadBalancers em regiões distintas.

5.1 Para criar uma zona privada será necessário associa-la a uma VPC, portanto vamos pegar o ID da nossa VPC onde subimos a nossa EC2 do Cloud9.

```bash
export VPC_ID=$(aws ec2 describe-instances --filters 'Name=tag:Name,Values=*workshop*' --region us-east-1 | jq '.Reservations[0].Instances[0].VpcId')

# Criar hosted zona na AWS associando a VPC da instancia do Cloud9

export ZONE_ID=$(aws route53 create-hosted-zone --name my.private.hz --vpc "VPCRegion=us-east-1,VPCId=${VPC_ID}" --hosted-zone-config 'Comment=private,PrivateZone=true' --caller-reference $(date "+%Y%m%d%H%M%S") | jq .HostedZone.Id | cut -d '/' -f3)
```

Agora que criamos nossa Zona DNS nós iremos configurar o failover ativo-passivo. Para mais informações sobre políticas de roteamento na AWS acesse [esta documentação](https://docs.aws.amazon.com/pt_br/Route53/latest/DeveloperGuide/routing-policy.html).

5.2 Vamos criar o health check para o nosso endpoint primário que será na **us-east-1**

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

5.3 Criando nosso endpoint primário utilizando política de roteamento Failover.

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

Aguarde alguns minutos para que o healthcheck esteja ativo.

5.4 Criando nosso endpoint secundário utilizando política de roteamento Failover.

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

Note que estamos associando o healthcheck apenas com o primário.

5.5 Resolver o DNS que criamos e verificar para qual endpoint de LoadBalancer está apontando (Como nós associamos a hosted zone com a VPC da instância do Cloud9 nós conseguimos resolver o DNS).

```bash
curl -XGET http://dr-app.my.private.hz
```

Se tudo estiver certo o resultado da região deve ser.

{{< output >}}
{
  "region": "us-east-1"
}
{{< /output >}}

#### 6. Simular falha da região primária us-east-1

6.1 Vamos atualizar o nosso `FederatedDeployment` fazendo com que o POD da nossa aplicação só exista na região `us-east-2`.

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

6.2 Vamos aplicar o nosso novo manifesto

```bash
kubectl apply -f federated_deployment.yaml
```

Aguarde alguns minutos até que a atualização de propague em nossa zona de DNS.

6.3 Execute o CURL novamente no nosso endpoint.

```bash
curl -XGET http://dr-app.my.private.hz
```

Se tudo estiver certo o resultado da região deve ser.

```json
{
  "region": "us-east-2"
}
```

#### 7. Apagando os recursos

7.1 Deletar o `FederatedDeployment` e `FederatedService` (Será deletado dos dois clusters).

```bash
kubectl delete -f federated_deployment.yaml
kubectl delete -f federated_service.yaml
```

7.2 Exclua o cluster e seus nós associados com o comando a seguir.

```bash
eksctl delete cluster --region=us-east-1 --name kubefed-cluster
eksctl delete cluster --region=us-east-2 --name kubefed-cluster
```
