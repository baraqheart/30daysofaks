why ingress and problems ingress solve

- exposing services to the outside world was complex, which is now possible with ingress through a 
  single entry point

- distributing traffic across multiple pods was difficult

- path based routing was difficult, now possible with ingress

- securing traffic with ssl/tls was cumbersome. with Ingress, security of cummunication is now handled by ingress

To use ingress service in our cluster we need an ingress controller there are 
several ingress controller, in this project we will use [nginx controller](https://kubernetes.github.io/ingress-nginx/deploy/) which support a large variety of providers 

check this [link](https://learn.microsoft.com/en-us/azure/aks/ingress-basic?tabs=azure-cli#create-an-ingress-controller) for azure aks provider documentation


STEPS


### Step 1: Create a Public IP

you can create a public IP using the portal or  the cli

```
az network public-ip create --resource-group <resouregrpnaem> --name <pulibip1>
--sku standard --allocation-method static --query publicIp.ipAddress -o tsv

```



### Step 2: Install Helm

- visit the [helm documetation page](https://helm.sh/docs/intro/install/) and install helm using the command below to install helm on ubuntun os 

```
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm

```

- verify that helm is install and go to the next step

` helm --version `

`helm help



### Step 3: install nginx ingress contoller using Helm

- visit this [page](https://kubernetes.github.io/ingress-nginx/deploy/s) to see a good list
of providers and choose your provider, for this project, we will be using azure.

- run the command below for a quick start of ingress on azure
```

kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.10.1/deploy/static/provider/cloud/deploy.yaml

```

- if you are interested in deploying an ingress controller with more custom values visit [aks docs](https://learn.microsoft.com/en-us/azure/aks/ingress-basic?tabs=azure-cli#create-an-ingress-controller)

```

# Add the ingress-nginx repository
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

# view what values are available before installing
helm show values ingress-nginx/ingress-nginx


# Use Helm to deploy an NGINX ingress controller
helm install ingress-nginx ingress-nginx/ingress-nginx \
    --version 4.7.1 \
    --namespace ingress-basic \
    --create-namespace \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."kubernetes\.io/os"=linux \
    --set controller.service.externalTrafficPolicy=Local \
    --set controller.service.loadBalancerIP=10.224.0.42 \
```

- verify that the controller has been installed

` kubectl get service -n ingress-basic `

### Step 4: Deploy the App 

- create a file and save it as app.yaml. copy the code and save the file

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aks-helloworld-one  
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aks-helloworld-one
  template:
    metadata:
      labels:
        app: aks-helloworld-one
    spec:
      containers:
      - name: aks-helloworld-one
        image: mcr.microsoft.com/azuredocs/aks-helloworld:v1
        ports:
        - containerPort: 80
        env:
        - name: TITLE
          value: "Welcome to Azure Kubernetes Service (AKS) with baraqheaart"
---
apiVersion: v1
kind: Service
metadata:
  name: aks-helloworld-svc  
spec:
  type: ClusterIP
  ports:
  - port: 80
  selector:
    app: aks-helloworld-one

---

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: hello-ingress
spec:
  ingressClassName: nginx
  rule:
  - http:
      paths:
      - path: /
        pathtype: Prefix
        backend: 
          service:
            name: aks-helloworld-svc
            port:
              number: 80


```

- create and deploy a simple application that uses ingress to expose its ip




### Step 5: Validate the App and cleanup




REFERENCES
[kubernetes documentation](https://kubernetes.io/docs/reference/)

[azure file storage docs for aks](https://learn.microsoft.com/en-us/azure/aks/azure-csi-files-storage-provision)

 [kubernetes api reference documentation](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.23/#-strong-api-overview-strong-)
