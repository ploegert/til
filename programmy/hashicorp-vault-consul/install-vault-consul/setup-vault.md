# Setup Vault

## Setup Variables

```text
$sa = "obtaz1vaultsa"
$rg = "obtaz1-core-rg"


# Change these four parameters as needed for your own environment
$sa=$sa
$rg=$rg
$loc="eastus2"
$AKS_PERS_SHARE_NAME="aksshare"

# Create a resource group
az group create --name $rg --location $loc
az storage account create -n $sa -g $rg -l $loc --sku Standard_LRS
$sa_connstr=$(az storage account show-connection-string -n $sa -g $rg -o tsv)
az storage share create -n $AKS_PERS_SHARE_NAME --connection-string $sa_connstr
$STORAGE_KEY=$(az storage account keys list --resource-group $rg --account-name $sa --query "[0].value" -o tsv)

# Echo storage account name and key
write-host "Storage account name: $sa"          # obtaz1vaultsa
write-host "Storage account key: $STORAGE_KEY"  # SOEMPASS

write-host "kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=$sa --from-literal=azurestorageaccountkey=$STORAGE_KEY"
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=$sa --from-literal=azurestorageaccountkey=$STORAGE_KEY



# To Init the operator
kubectl exec -it vault-0 -- vault status
$b = kubectl exec -ti vault-0 -- vault operator init -format "json"
$c = $b | convertfrom-json
kubectl exec -it vault-0 -- vault status

write-host "$c.root_token"  

# Uninstall the vault stack
$sa = "obtaz1vaultsa"
$rg = "obtaz1-core-rg"

helm uninstall vault
kubectl delete pvc data-vault-0
kubectl delete pvc data-vault-1
kubectl delete pvc data-vault-2
kubectl delete pv data-vault-0
kubectl delete pv data-vault-1
kubectl delete pv data-vault-2
$sa_connstr=$(az storage account show-connection-string -n $sa -g $rg -o tsv)
az storage share delete  -n "vault0" --connection-string $sa_connstr
az storage share delete  -n "vault1" --connection-string $sa_connstr
az storage share delete  -n "vault2" --connection-string $sa_connstr
kubectl delete secrets hashivault

# Check config
kubectl edit configmap vault-config
```

## Change these four parameters as needed for your own environment

```text
$sa = "obtaz1vaultsa"
$rg = "obtaz1-core-rg"


# Change these four parameters as needed for your own environment
$sa=$sa
$rg=$rg
$loc="eastus2"
$AKS_PERS_SHARE_NAME="aksshare"

# Create a resource group
az group create --name $rg --location $loc
az storage account create -n $sa -g $rg -l $loc --sku Standard_LRS
$sa_connstr=$(az storage account show-connection-string -n $sa -g $rg -o tsv)
az storage share create -n $AKS_PERS_SHARE_NAME --connection-string $sa_connstr
$STORAGE_KEY=$(az storage account keys list --resource-group $rg --account-name $sa --query "[0].value" -o tsv)

# Echo storage account name and key
write-host "Storage account name: $sa"          # obtaz1vaultsa
write-host "Storage account key: $STORAGE_KEY"  # SOEMPASS

write-host "kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=$sa --from-literal=azurestorageaccountkey=$STORAGE_KEY"
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=$sa --from-literal=azurestorageaccountkey=$STORAGE_KEY



# To Init the operator
kubectl exec -it vault-0 -- vault status
$b = kubectl exec -ti vault-0 -- vault operator init -format "json"
$c = $b | convertfrom-json
kubectl exec -it vault-0 -- vault status

write-host "$c.root_token"  

# Uninstall the vault stack
$sa = "obtaz1vaultsa"
$rg = "obtaz1-core-rg"

helm uninstall vault
kubectl delete pvc data-vault-0
kubectl delete pvc data-vault-1
kubectl delete pvc data-vault-2
kubectl delete pv data-vault-0
kubectl delete pv data-vault-1
kubectl delete pv data-vault-2
$sa_connstr=$(az storage account show-connection-string -n $sa -g $rg -o tsv)
az storage share delete  -n "vault0" --connection-string $sa_connstr
az storage share delete  -n "vault1" --connection-string $sa_connstr
az storage share delete  -n "vault2" --connection-string $sa_connstr
kubectl delete secrets hashivault

# Check config
kubectl edit configmap vault-config
```

## Create a resource group



```text
# Create a resource group
az group create --name $rg --location $loc
az storage account create -n $sa -g $rg -l $loc --sku Standard_LRS
$sa_connstr=$(az storage account show-connection-string -n $sa -g $rg -o tsv)
az storage share create -n $AKS_PERS_SHARE_NAME --connection-string $sa_connstr
$STORAGE_KEY=$(az storage account keys list --resource-group $rg --account-name $sa --query "[0].value" -o tsv)
```

## Echo storage account name and key

```text
write-host "kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=$sa --from-literal=azurestorageaccountkey=$STORAGE_KEY"
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=$sa --from-literal=azurestorageaccountkey=$STORAGE_KEY
```



## To Init the operator

```text
# To Init the operator
kubectl exec -it vault-0 -- vault status
$b = kubectl exec -ti vault-0 -- vault operator init -format "json"
$c = $b | convertfrom-json
kubectl exec -it vault-0 -- vault status

write-host "$c.root_token"  
```



## Uninstall the vault stack

```text
helm uninstall vault
kubectl delete pvc data-vault-0
kubectl delete pvc data-vault-1
kubectl delete pvc data-vault-2
kubectl delete pv data-vault-0
kubectl delete pv data-vault-1
kubectl delete pv data-vault-2
$sa_connstr=$(az storage account show-connection-string -n $sa -g $rg -o tsv)
az storage share delete  -n "vault0" --connection-string $sa_connstr
az storage share delete  -n "vault1" --connection-string $sa_connstr
az storage share delete  -n "vault2" --connection-string $sa_connstr
kubectl delete secrets hashivault

# Check config
kubectl edit configmap vault-config
```



## Check config

```text
# Check config
kubectl edit configmap vault-config
```

kubectl edit configmap vault-config

