# Loading Credentials

## How to extract K8s credentials from azure

Terraform can be used executed locally using your own env, in a cloud shell via azure, or even via a docker image. This explanation will go through setting up natively on your own posh shell.

## Loading creds

Once the cluster is deployed, if you want to connect to the cluster, cd /app.Deploy/helm

```text
#=============================================
# Set the variables for your environment
$app = "app"
$env = "dev"
$zone = "z1"
$prefix = "$($app)$($env)$($zone)
$rg = "$prefix-core-rg"
$aks = "$prefix-core-aks"

# Load credentials for the cluster
az aks get-credentials --resource-group $rg --name $aks
kubectl config set-context --current --namespace=$ns

#=============================================

# Az commandline - set active subscription
az account set --subscription $subscription_id
az aks browse --resource-group $rg --name $aks

#=============================================
```

* 
This now means I have saved the connection info to my cluster into my ~.kube\config file. I can now test the connection by running the following:

#### 

#### 

