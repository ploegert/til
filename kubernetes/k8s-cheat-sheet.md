# K8S Cheat Sheet

#### Quick Reference 

```text
#verify health of the cluster
kubectl get nodes

# List all deployments in all namespaces
kubectl get deployments --all-namespaces=true

# List all deployments in a specific namespace
# Format :kubectl get deployments --namespace <namespace-name>
kubectl get deployments --namespace kube-system

# List details about a specific deployment
# Format :kubectl describe deployment <deployment-name> --namespace <namespace-name>
kubectl describe deployment my-dep --namespace kube-system

# List pods using a specific label
# Format :kubectl get pods -l <label-key>=<label-value> --all-namespaces=true
kubectl get pods -l app=nginx --all-namespaces=true

# Get logs for all pods with a specific label
# Format :kubecl logs -l <label-key>=<label-value>
kubectl logs -l app=nginx --namespace kube-system

# set default Namespace context
kubectl config set-context --current --namespace=app-d

# List the pods
kubectl get pods -l app.kubernetes.io/name=vault

# Connect to the instance
kubectl exec -ti vault-0 -- vault operator init

# Create a namespace for your ingress resources
kubectl create namespace app-d

#Get status:
kubectl --namespace ingress-basic get services -o wide -w nginx-ingress-controller
kubectl get service -l app=nginx-ingress --namespace ingress-basic
kubectl get events -n app-d
kubectl.exe logs kyron-api-948458ffc-27b65

#Create two new deployments 
kubectl apply -f ..\..\helm\app1.yaml --namespace app-d
kubectl apply -f ..\..\helm\app2.yaml --namespace app-d
kubectl apply -f ..\..\helm\api.yaml --namespace app-d

# Set Default Context
kubectl config set-context --current --namespace=app-d

# Delete deployments, service, and ingress
kubectl delete ingress kyron-api
kubectl.exe delete deployments kyron-api
kubectl.exe delete service kyron-api

#uninstall:
helm delete consul
kubectl delete namespace consul
```

#### Resources

* [Getting started with k8s](https://learnk8s.io/blog/get-start-terraform-aks)
* [Overview of kubectl](https://go.microsoft.com/fwlink/?linkid=2135816)
* [Getting started with kubectl](https://go.microsoft.com/fwlink/?linkid=2135746)
* [Kubectl cheat sheet](https://go.microsoft.com/fwlink/?linkid=2135748)
* [Kubectl reference](https://go.microsoft.com/fwlink/?linkid=2135818)

#### 

### To install the K8s Dasbhoard:

```text
kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard

az aks enable-addons --addons kube-dashboard -g $rg -n $aks
```

### Additional way to export the k8s config - this didn't work for me tho :\(

```text
# push output config from terraform to a file
echo "$(terraform output kube_config)" > ./azurek8s

# setup env variable for k8s
$env:KUBECONFIG = "./azurek8s"
```

```text
$sub  = "SOME_SUB"
$tenant  = "SOME_AAD_TENANT"

az ad sp create-for-rbac --subscription $sub --sdk-auth | base64 -w0
```

### Creating Ingres using NGinx

```text
# Create a namespace for your ingress resources
kubectl create namespace app-d

# Add the official stable repository
helm repo add stable https://kubernetes-charts.storage.googleapis.com/

# Use Helm to deploy an NGINX ingress controller
helm install nginx-ingress stable/nginx-ingress `
    --namespace app-d `
    -f ..\..\helm\internal-ingress.yaml `
    --set controller.replicaCount=2 `
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux `
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux


# HELM
kubectl create namespace consul

helm install consul hashicorp/consul `
  --namespace consul  `
  -f ..\..\helm\hc-consul.yaml  `
  --set connectInject.enabled=true `
  --set connectInject.nodeSelector="beta.kubernetes.io/os: linux" `
  --set client.enabled=true `
  --set client.grpc=true `
  --set client.nodeSelector="beta.kubernetes.io/os: linux" `
  --set server.nodeSelector="beta.kubernetes.io/os: linux" `
  --set syncCatalog.enabled=true `
  --set syncCatalog.nodeSelector="beta.kubernetes.io/os: linux"

kubectl port-forward -n consul svc/consul-consul-ui 8080:80



kubectl create secret tls first-secret-name \
  --cert first-cert-file --key first-key-file



#Get status:
kubectl --namespace ingress-basic get services -o wide -w nginx-ingress-controller
kubectl get service -l app=nginx-ingress --namespace ingress-basic
kubectl get events -n ingress-basic

kubectl get events -n app-d

#Create two new deployments 
kubectl apply -f ..\..\helm\app1.yaml --namespace app-d
kubectl apply -f ..\..\helm\app2.yaml --namespace app-d

# K8s for
kubectl config set-context --current --namespace=app-d
kubectl apply -f ..\..\helm\api.yaml --namespace app-d
kubectl apply -f ..\..\helm\api2.yaml --namespace app-d


kubectl apply -f ..\..\helm\justin.yaml --namespace app-d

# helm install api ..\..\helm\api.yaml --namespace app-d


kubectl exec -it api-948458ffc-m5dml -- curl http://localhost:5000/status

kubectl exec -it api-948458ffc-m5dml -- curl http://172.16.192.32:80/status

$pod = "justin-7fdc8596bb-pmk29"
kubectl exec -it $pod -- curl http://172.16.192.32:80/status
kubectl exec -it $pod -- curl http://localhost:5000/status

kubectl logs --tail=20 api-948458ffc-tfvkl

kubectl logs -f $(kubectl get pods | awk '/api/ {print $1;exit}')


kubectl delete ingress api
kubectl.exe delete deployments api
kubectl.exe delete service api

$name = "justin"
kubectl delete ingress $name
kubectl.exe delete deployments $name
kubectl.exe delete service $name

#uninstall:
helm delete consul
kubectl delete namespace consul



$ACR_SHORTNAME = "SOMEREPO"
$ACR_NAME="SOMEREPO.azurecr.io"
$ACR_UNAME="appd-creds-topull"
$ACR_PASSWD = "SOMEPASSWORD"

# assumes ACR Admin Account is enabled
ACR_UNAME=$(az acr credential show -n $ACR_NAME --query="username" -o tsv)
ACR_PASSWD=$(az acr credential show -n $ACR_NAME --query="passwords[0].value" -o tsv)
kubectl create secret docker-registry $ACR_SHORTNAME `
  --docker-server=$ACR_NAME `
  --docker-username=$ACR_UNAME `
  --docker-password=$ACR_PASSWD `
  --docker-email=cploegj@jci.com


docker login -u appd-creds-topull -p SOMEPASS SOMEREPO.azurecr.io


#Create the ingres route in the hello-world-ingress.yaml, then load
kubectl apply -f ..\..\helm\hello-world-ingress.yaml

# Test: To test the routes for the ingress controller, browse to the two applications with a web client. If needed, you can quickly test this internal-only functionality from a pod on the AKS cluster. Create a test pod and attach a terminal session to it:

#either or
kubectl run -it --rm aks-ingress-test --image=debian --namespace ingress-basic
kubectl attach aks-ingress-test-cc78684bb-t482j -c aks-ingress-test -i -t

# Install curl in the pod using apt-get:
apt-get update && apt-get install -y curl

# Now access the address of your Kubernetes ingress controller using curl, such as http://172.16.192.245. Provide your own internal IP address specified when you deployed the ingress controller in the first step of this article.
curl -L http://172.16.192.245

```

## Create a namespace for your ingress resources

```text
kubectl create namespace app-d
```

## Add the official stable repository

```text
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```

## Use Helm to deploy an NGINX ingress controller

```text
helm install nginx-ingress stable/nginx-ingress `
    --namespace app-d `
    -f ..\..\helm\internal-ingress.yaml `
    --set controller.replicaCount=2 `
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux `
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

## HELM

```text
kubectl create namespace consul

helm install consul hashicorp/consul `
  --namespace consul  `
  -f ..\..\helm\hc-consul.yaml  `
  --set connectInject.enabled=true `
  --set connectInject.nodeSelector="beta.kubernetes.io/os: linux" `
  --set client.enabled=true `
  --set client.grpc=true `
  --set client.nodeSelector="beta.kubernetes.io/os: linux" `
  --set server.nodeSelector="beta.kubernetes.io/os: linux" `
  --set syncCatalog.enabled=true `
  --set syncCatalog.nodeSelector="beta.kubernetes.io/os: linux"

kubectl port-forward -n consul svc/consul-consul-ui 8080:80

kubectl create secret tls first-secret-name \
  --cert first-cert-file --key first-key-file
```

## Get status:

```text
kubectl --namespace ingress-basic get services -o wide -w nginx-ingress-controller
kubectl get service -l app=nginx-ingress --namespace ingress-basic
kubectl get events -n ingress-basic

kubectl get events -n app-d
```

## Create two new deployments

```text
kubectl apply -f ..\..\helm\app1.yaml --namespace app-d
kubectl apply -f ..\..\helm\app2.yaml --namespace app-d
```



## K8s for

```text

kubectl apply -f ..\..\helm\api.yaml --namespace app-d
kubectl apply -f ..\..\helm\api2.yaml --namespace app-d


kubectl apply -f ..\..\helm\justin.yaml --namespace app-d

# helm install api ..\..\helm\api.yaml --namespace app-d


kubectl exec -it api-948458ffc-m5dml -- curl http://localhost:5000/status

kubectl exec -it api-948458ffc-m5dml -- curl http://172.16.192.32:80/status

$pod = "justin-7fdc8596bb-pmk29"
kubectl exec -it $pod -- curl http://172.16.192.32:80/status
kubectl exec -it $pod -- curl http://localhost:5000/status

kubectl logs --tail=20 api-948458ffc-tfvkl

kubectl logs -f $(kubectl get pods | awk '/api/ {print $1;exit}')
```



## uninstall:

```text
kubectl delete ingress api
kubectl.exe delete deployments api
kubectl.exe delete service api

$name = "justin"
kubectl delete ingress $name
kubectl.exe delete deployments $name
kubectl.exe delete service $name

#uninstall:
helm delete consul
kubectl delete namespace consul

```

## Create the ingres route in the hello-world-ingress.yaml, then load

```text
kubectl apply -f ....\helm\hello-world-ingress.yaml
```

## Install curl in the pod using apt-get:

```text
apt-get update && apt-get install -y curl
```

## Access the Containers:

```text
# assumes ACR Admin Account is enabled
ACR_UNAME=$(az acr credential show -n $ACR_NAME --query="username" -o tsv)
ACR_PASSWD=$(az acr credential show -n $ACR_NAME --query="passwords[0].value" -o tsv)
kubectl create secret docker-registry $ACR_SHORTNAME `
  --docker-server=$ACR_NAME `
  --docker-username=$ACR_UNAME `
  --docker-password=$ACR_PASSWD `
  --docker-email=cploegj@jci.com


docker login -u appd-creds-topull -p SOMEPASS SOMEREPO.azurecr.io


#Create the ingres route in the hello-world-ingress.yaml, then load
kubectl apply -f ..\..\helm\hello-world-ingress.yaml

# Test: To test the routes for the ingress controller, browse to the two applications with a web client. If needed, you can quickly test this internal-only functionality from a pod on the AKS cluster. Create a test pod and attach a terminal session to it:

#either or
kubectl run -it --rm aks-ingress-test --image=debian --namespace ingress-basic
kubectl attach aks-ingress-test-cc78684bb-t482j -c aks-ingress-test -i -t

# Install curl in the pod using apt-get:
apt-get update && apt-get install -y curl

# Now access the address of your Kubernetes ingress controller using curl, such as http://172.16.192.245. Provide your own internal IP address specified when you deployed the ingress controller in the first step of this article.
curl -L http://172.16.192.245
```

## 

### AKS

* [https://learn.hashicorp.com/terraform/kubernetes/provision-aks-cluster](https://learn.hashicorp.com/terraform/kubernetes/provision-aks-cluster)
* [https://docs.microsoft.com/en-us/azure/developer/terraform/create-k8s-cluster-with-aks-applicationgateway-ingress](https://docs.microsoft.com/en-us/azure/developer/terraform/create-k8s-cluster-with-aks-applicationgateway-ingress)
* [https://docs.microsoft.com/en-us/azure/aks/ingress-internal-ip](https://docs.microsoft.com/en-us/azure/aks/ingress-internal-ip)

### Alternative - use App Gateway Ingress Controller \(AGIC\)

* [https://docs.microsoft.com/en-us/azure/application-gateway/tutorial-ingress-controller-add-on-new](https://docs.microsoft.com/en-us/azure/application-gateway/tutorial-ingress-controller-add-on-new)
* [https://docs.microsoft.com/en-us/azure/application-gateway/ingress-controller-overview](https://docs.microsoft.com/en-us/azure/application-gateway/ingress-controller-overview)
* [https://kubernetes.io/docs/concepts/services-networking/ingress/](https://kubernetes.io/docs/concepts/services-networking/ingress/)
* [https://docs.microsoft.com/en-us/azure/aks/use-managed-identity](https://docs.microsoft.com/en-us/azure/aks/use-managed-identity)

### Lets Encrypt

* [https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04)
* [https://opencredo.com/blogs/letsencrypt-terraform/](https://opencredo.com/blogs/letsencrypt-terraform/)
* [https://ege.dev/post/renew-ssl-certs-in-azure-app-gateway-using-gitlabci/](https://ege.dev/post/renew-ssl-certs-in-azure-app-gateway-using-gitlabci/)
* [https://docs.microsoft.com/en-us/azure/application-gateway/ssl-overview\#end-to-end-ssl-with-the-v2-sku](https://docs.microsoft.com/en-us/azure/application-gateway/ssl-overview#end-to-end-ssl-with-the-v2-sku)
* [https://docs.microsoft.com/en-us/azure/application-gateway/end-to-end-ssl-portal](https://docs.microsoft.com/en-us/azure/application-gateway/end-to-end-ssl-portal)
* [https://github.com/vancluever/terraform-provider-acme](https://github.com/vancluever/terraform-provider-acme)

