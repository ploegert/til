---
description: This document calls out getting started with Terraform
---

# Hashicorp Terraform

## 1: Ensure we install all the pre-reqs on your station.

Pre-Reqs

* Terraform
* Azure CLI
* psql
* Kubectl
* helm
* consul
* vault
* openssl

#### Installation of Terraform & Azure CLI

Make sure you install terraform from here: [https://www.terraform.io/downloads.html](https://www.terraform.io/downloads.html).

**On Windows**

I like [chococately](https://chocolatey.org/), so you can install using:

```csharp
# install choco
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

# Powershell
choco install pwsh -y

# Aspnetcore
choco install dotnetcore-windowshosting -y
choco install dotnetcore -y
choco install dotnetcore-2.0-sdk -y
choco install dotnetcore-2.1-sdk -y
choco install dotnetcore-2.2-sdk -y
choco install dotnetcore-3.0-sdk -y
choco install dotnetcore-3.1-sdk -y
choco install dotnetcore3-desktop-runtime -y
choco install dotnetcore-3.1-runtime -y
choco install dotnet-5.0-runtime -y
choco install dotnet-5.0-sdk -y
choco install netfx-4.8 -y
choco install dotnetfx -y

# install Azure CLI via choco
choco install azure-cli -y
az extension add --name azure-cli-iot-ext

# PSQL:
choco install postgresql12 -y

# Hashi Consul/Vault/Terraform
choco install consul -y
choco install choco install vault -y
choco install terraform -y

# docker/Kubernetes/helm
choco install docker-desktop -y
choco install kubernetes-cli -y
choco install kubernetes-helm -y

# Python
choco install python -y

# node
choco install nodejs -y

```

**On Ubuntu**

```bash
sudo apt update && apt upgrade -y

# install Terraform
wget https://releases.hashicorp.com/terraform/0.14.6/terraform_0.14.6_linux_amd64.zip
sudo apt-get install zip -y
unzip terraform*.zip
sudo mv terraform /usr/local/bin
terraform version

# Install AZ Cli & extensions
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
az extension add --name azure-cli-iot-ext
az extension add --name azure-iot

# install psql
sudo apt-get update
sudo apt-get install postgresql-client

# Install Kubectl
sudo apt-get update && sudo apt-get install -y apt-transport-https gnupg2
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl

# Install Helm
curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
sudo apt-get install apt-transport-https --yes
echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm

# Or install using scripts:
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh

# Install Vault
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install vault


wget https://releases.hashicorp.com/vault/1.5.0/vault_1.5.0_linux_amd64.zip
unzip vault_1.5.0_linux_amd64.zip
sudo mv vault /usr/bin
vault


# Install Consul
wget https://releases.hashicorp.com/consul/1.8.2/consul_1.8.2_linux_amd64.zip
unzip consul_1.8.2_linux_amd64.zip
sudo mv consul /usr/bin
consul

# install ansible
sudo apt update
sudo apt install software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install ansible

# install Pip
sudo apt install python3-pip
pip3 install 'ansible[azure]'

curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3 get-pip.py --user

# python dependencies
sudo apt-get install build-essential checkinstall
sudo apt-get install libreadline-gplv2-dev libncursesw5-dev libssl-dev \
    libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev zlib1g-dev

sudo apt install python3.9

# Install SDK
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

#### Setup your Env Variables with SPN for Azure

In order to use terraform in a secure way, the spn id/secrets should not be stored in source code, so each time you want to setup an env, you'll want to change your env variables to the necessary accounts that have access. Here is an example way of using powershell to create the evn settings

**Windows**

```text
$env:ARM_CLIENT_ID  = "SOME_ID"
$env:ARM_CLIENT_SECRET    = "SOME_PASSWORD"
$env:ARM_SUBSCRIPTION_ID  = "SOME_SUBSCRIPTION"
$env:ARM_TENANT_ID        = "SOME_AAD_CLIENT"
```

**Ubunut**

```bash
export ARM_CLIENT_ID="SOME_ID"
export ARM_CLIENT_SECRET="SOME_PASSWORD"
export ARM_SUBSCRIPTION_ID="SOME_SUBSCRIPTION"
export ARM_TENANT_ID="SOME_AAD_CLIENT"
```

#### Install openssl

```text
choco install openssl -y
```

## 2: Setup your environment variables:

In order to use terraform in a secure way, the spn id/secrets should not be stored in source code, so each time you want to setup an env, you'll want to change your env variables to the necessary accounts that have access. Here is an example way of using powershell to create the evn settings

#### Windows

```text
  $env:ARM_CLIENT_ID = "SOME_ID"
  $env:ARM_CLIENT_SECRET = "SOME_PASSWORD"
  $env:ARM_SUBSCRIPTION_ID  = "SOME_SUBSCRIPTION"
  $env:ARM_TENANT_ID  = "SOME_AAD_TENANT"
```

#### Ubuntu

```bash
  export ARM_CLIENT_ID="SOME_ID"
  export ARM_CLIENT_SECRET="SOME_PASSWORD"
  export ARM_SUBSCRIPTION_ID="SOME_SUBSCRIPTION"
  export ARM_TENANT_ID="SOME_AAD_TENANT""
```

## 3: Generate a ssh key to be used for securing the deployment: <a id="3-generate-a-ssh-key-to-be-used-for-securing-the-deployment"></a>

```text
# If windows:
Set-location ~ 
mkdir ~/.ssh

Ssh-keygen.exe


#      Generating public/private rsa key pair.
#      Enter file in which to save the key (/c/Users/joetest/.
#       ssh/id_rsa): /c/Users/joetest/.ssh/
#      Enter passphrase (empty for no passphrase):
#      Enter same passphrase again:
#      Your identification has been saved in /c/Users/joetest/
#      .ssh/
#      Your public key has been saved in /c/Users/joetest/.ssh/
#      The key fingerprint is:
#      #SHA256:jieniOIn20935n0awtn04n002HqEIOnTIOnevHzaI5nak
#      joetest@periwinkle
#      The key's randomart image is:

#      +---[RSA 2048]----+
#      |*o =.            |
#      |O*=*=.B       +-3|
#      |+* +             |
#      |    +   #$^  4   |
#      |             +.  |
#      | .o.ooo* o       |
#      |  .+o .          |
#      |   .=.           |
#      |   Eo      +*oo  |
#      +----[SHA256]-----+

#      $ dir .ssh
#      id_rsa  id_rsa.pub

$pub_key = get-content ~/.ssh/id_rsa.pub
```

‌Once this is done, then add the public key‌

#### Convert root cert token for API gateway to base64

> Note: This only needs to be done once, adn then update vnet\_gw\_vpn\_cert\_name & vnet\_gw\_vpn\_cert\_data in the /terraform/base-env-setup/main.tf

```text
$path = "./certs/appdz1-root-public.cer"$file = [System.Text.Encoding]::UTF8.GetBytes((get-content ($path)))$certText = [Convert]::ToBase64String($file)
```

## ​4: Setup your Workspace

Your terraform workspace is important, as it tells the terraform which variables to use for an associated environment, as well as maintaining the state for what is deployed. For the most part, the default settings can be the same across all environments - but you'll want to differentiate the name/location of the resources that are deployed. This is accomplished using the Terraform Workspace.

> Reference: [Terraform Workspaces](https://www.terraform.io/docs/state/workspaces.html)

```text
# Initialize your new workspace
terraform workspace new delta

    Created and switched to workspace "delta"!

    You're now on a new, empty workspace. Workspaces isolate their state,
    so if you run "terraform plan" Terraform will not see any existing state
    for this configuration.
```

Once you have done this, you'll want to ensure that you update the [terraform/base-env-setup/main.tf](./terraform/base-env-setup/main.tf) to include new environment variables. The idea is we'll have a few "placeholders" for transient environments such as:

* Dev
* Alpha
* Bravo
* Charlie

If you wanted to add say "Delta", you'd go to the[ terraform/base-env-setup/main.tf](./terraform/base-env-setup/main.tf) file and add a new section after Charlie -- ensuring you add whatever customization you need for variables to make your new env unique to you.

```javascript
// Note Truncated for brevity
   dev = {

      ssl_certificate_name          = "dev.domain.com"
      ssl_certificate_data          = "MI..0A=="
      ssl_certificate_password      = "SOMEPASSWORD"
    }

    alpha = {

      ssl_certificate_name          = "dev.domain.com"
      ssl_certificate_data          = "MI..0A=="
      ssl_certificate_password      = "SOMEPASSWORD"
    }

    bravo = {

      ssl_certificate_name          = "dev.domain.com"
      ssl_certificate_data          = "MI..0A=="
      ssl_certificate_password      = "SOMEPASSWORD"
    }

    charlie = {

      ssl_certificate_name          = "dev.domain.com"
      ssl_certificate_data          = "MI..0A=="
      ssl_certificate_password      = "SOMEPASSWORD"
    }

    // insert new env here
```

#### Setup your workspace

To ensure you connect to the right state store in the blob, make sure you are configured to the proper env

```text
# to see what workspaces exist on your station:
terraform workspace list

# to create a new workspace:
terraform workspace new dev

# if you have existing workspaces, and want to set to a specific one:
terraform workspace select dev
```

## 5: Setting up Remote State for Terrafrom

In order to save state to a remote storage location \(azure blob\), we'll need to setup a storage account and some containers. You'll do that by executing the terraform script under /terraform/1-tf-remotestate.

NOTE: If this has already been executed, you don't need to re-run this 1-tf-remotestate. This exists for the new appdz1 env!

```text
# Put yourself in the root of the terraform folder (not the module level - the root l evel)
set-location "./terraform/1-tf-remotestate"

# Type terraform validate to validate the config
terraform validate

#Output
Success! The configuration is valid.
```



## 6: Run the initial Terraform deploy of shared stuffs:

```text
  $basepath = "c:/vso"
  set-location $basepath/app.Deploy/terraform/base-env-setup

  # Verify that you'vev downloaded all the terraform provider
  terraform init

  # Verify that everything builds
  terraform validate

  # Make it so!
  terraform apply
```

By now, we should have the following deployed:

* Shared Module:
  * Core RG
  * Vnet & subnets
  * AKS Cluster
  * App Gateway & PIP
  * Traffic Manager \(For DNS name\)
  * Log Analytics

Another Example output:

```text
set-location "./terraform/1-tf-remotestate"

# see what would happen without actually deploying
terraform plan

# apply the configuration
terraform apply

...

# Example output
module.api.azurerm_postgresql_virtual_network_rule.pgsql: Refreshing state... [id=/subscriptions/SOME_SUB_ID/resourceGroups/SOME_RG_1/providers/Microsoft.DBforPostgreSQL/servers/SOME_SVC-sql/virtualNetworkRules/env-openshift]
module.api.azurerm_resource_group.resource_group: Refreshing state... [id=/subscriptions/SOME_SUB_ID/resourceGroups/SOME_RG_1]
module.api.azurerm_postgresql_server.pgsql: Refreshing state... [id=/subscriptions/SOME_SUB_ID/resourceGroups/SOME_RG_1/providers/Microsoft.DBforPostgreSQL/servers/SOME_SVC-sql]
module.api.azurerm_postgresql_database.db: Refreshing state... [id=/subscriptions/SOME_SUB_ID/resourceGroups/SOME_RG_1/providers/Microsoft.DBforPostgreSQL/servers/SOME_SVC-sql/databases/SOME_SVC-db]

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create
  - destroy

Terraform will perform the following actions:

  # module.api.azurerm_postgresql_virtual_network_rule.pgsql will be destroyed
  - resource "azurerm_postgresql_virtual_network_rule" "pgsql" {
      - id                                   = "/subscriptions/SOME_SUB_ID/resourceGroups/SOME_RG_1/providers/Microsoft.DBforPostgreSQL/servers/SOME_SVC-sql/virtualNetworkRules/env-openshift" -> null
      - ignore_missing_vnet_service_endpoint = true -> null
      - name                                 = "env-openshift" -> null
      - resource_group_name                  = "SOME_RG_1" -> null 
      - server_name                          = "SOME_SVC-sql" -> null
      - subnet_id                            = "/subscriptions/SOME_SUB_ID/resourceGroups/env-msdn-vnet-rg/providers/Microsoft.Network/virtualNetworks/env-msdn-vnet/subnets/env-openshift-subnet" -> null
    }

  # module.api.azurerm_postgresql_virtual_network_rule.pgsql_rule1 will be created
  + resource "azurerm_postgresql_virtual_network_rule" "pgsql_rule1" {
      + id                                   = (known after apply)
      + ignore_missing_vnet_service_endpoint = true
      + name                                 = "env-openshift"  
      + resource_group_name                  = "SOME_RG_1"
      + server_name                          = "SOME_SVC-sql"
      + subnet_id                            = "/subscriptions/SOME_SUB_ID/resourceGroups/env-msdn-vnet-rg/providers/Microsoft.Network/virtualNetworks/env-msdn-vnet/subnets/env-openshift-subnet"
    }

  # module.api.azurerm_postgresql_virtual_network_rule.pgsql_rule2 will be created
  + resource "azurerm_postgresql_virtual_network_rule" "pgsql_rule2" {
      + id                                   = (known after apply)    
      + ignore_missing_vnet_service_endpoint = true
      + name                                 = "devqa-VPN-vnet"
      + resource_group_name                  = "SOME_RG_1"
      + server_name                          = "SOME_SVC-sql"
      + subnet_id                            = "/subscriptions/MGMT_SUBSCRIPTION/resourceGroups/mgmt-devqa-vpn-vnet-rg/providers/Microsoft.Network/virtualNetworks/mgmt-devqa-vpn-vnet/subnets/vpn-subnet"
    }

Plan: 2 to add, 0 to change, 1 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

#type yes if you are good with the config
  Enter a value: yes

module.api.azurerm_postgresql_virtual_network_rule.pgsql: Destroying... [id=/subscriptions/SOME_SUB_ID/resourceGroups/SOME_RG_1/providers/Microsoft.DBforPostgreSQL/servers/SOME_SVC-sql/virtualNetworkRules/env-openshift]
module.api.azurerm_postgresql_virtual_network_rule.pgsql_rule1: Creating...
module.api.azurerm_postgresql_virtual_network_rule.pgsql_rule2: Creating...
module.api.azurerm_postgresql_virtual_network_rule.pgsql: Destruction complete after 2s
module.api.azurerm_postgresql_virtual_network_rule.pgsql_rule2: Still creating... [20s elapsed]
module.api.azurerm_postgresql_virtual_network_rule.pgsql_rule1: Still creating... [30s elapsed]
module.api.azurerm_postgresql_virtual_network_rule.pgsql_rule2: 

# Truncated log

module.api.azurerm_postgresql_virtual_network_rule.pgsql_rule1: Still creating... [2m20s elapsed]
module.api.azurerm_postgresql_virtual_network_rule.pgsql_rule2: Still creating... [2m20s elapsed]
module.api.azurerm_postgresql_virtual_network_rule.pgsql_rule2: Creation complete after 2m23s [id=/subscriptions/SOME_SUB_ID/resourceGroups/SOME_RG_1/providers/Microsoft.DBforPostgreSQL/servers/SOME_SVC-sql/virtualNetworkRules/devqa-VPN-vnet]
module.api.azurerm_postgresql_virtual_network_rule.pgsql_rule1: Still creating... [2m30s elapsed]
module.api.azurerm_postgresql_virtual_network_rule.pgsql_rule1: Still creating... [2m40s elapsed]                                                                           
module.api.azurerm_postgresql_virtual_network_rule.pgsql_rule1: Still creating... [2m50s elapsed]
module.api.azurerm_postgresql_virtual_network_rule.pgsql_rule1: Creation complete after 2m54s [id=/subscriptions/SOME_SUB_ID/resourceGroups/SOME_RG_1/providers/Microsoft.DBforPostgreSQL/servers/SOME_SVC-sql/virtualNetworkRules/env-openshift]
                                                                                                                                                                                  
Apply complete! Resources: 2 added, 0 changed, 1 destroye
```

## 7. Run the Terraform DB Schema

Now we will need to manually add the firewall rules to allow the aks cluster to reach the pgsql.

1. Open the [https://portal.azure.com](https://portal.azure.com), and select that resource group for appdz1-core-rg.
2. Select the appdz1-core-hyperscale server group
3. Then select the Networking from the Settings category on the bottom left
4. There is an item that says "Allow Azure services and resources to access this server group". Change this selection from no to YES
5. You'll need to also add the public ip of your machine to the firewall rule, otherwise your schema scripts will fail. you can find out your ip address by typing "whats my ip" in google search.

Now that we have configured the access firewalls, we can configure run the schema scripts.

```text
set-location $basepath/terraform/product.db

  # Verify that you'vev downloaded all the terraform provider
  terraform init

  # Verify that everything builds
  terraform validate

  # Make it so!
  terraform apply
```

## 8: Export the k8s credentials:

in order to connect to the cluster, you'll need to credentials of what you just deployed. To do that, lets use AKS creds.

> Note: Executing this command is using the environment variables we loaded under step 2 as the user ak cli is running under

```text
$rg = "rg"
$aks = "core-aks"
az aks get-credentials --resource-group $rg --name $aks 


$env = "a"
$app = "app"
$zone = "z1"
$rg = "$($app)$($env)$($zone)-core-rg"
$aks = "$($app)$($env)$($zone)-core-aks"
az aks get-credentials --resource-group $rg --name $aks
```

## 9: Generate Secrets used for ingress

```text
# Generate new self signed cert to use for TLS within the cluster
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout "./certs/ingress.key" -out "./certs/ingress.pem" -subj "/CN=ingresstls/O=org"

# Insert certs into AKS
$app = "app"
$env = "d"
$namespace = "$($app)-$($env)"

$secretname = "ingresstls"
kubectl create secret tls $secretname `
    --namespace $namespace `
    --key "./certs/ingress.key" `
    --cert "./certs/ingress.pem"

# Insert secrets allowing our service to pull from ACR
$ACR_SHORTNAME = "SOME_REPO_NAME"
$ACR_NAME="SOME_REPO_NAME.azurecr.io"
$ACR_UNAME="$namespace-creds-topull"
$ACR_PASSWD = "SOMEPASSWORD"

kubectl create secret docker-registry $ACR_SHORTNAME `
  --docker-server=$ACR_NAME `
  --docker-username=$ACR_UNAME `
  --docker-password=$ACR_PASSWD `
  --docker-email="SOME_EMAIL"
```

## 10: Deploy the API

```text
$ns = "SOME_NS"

# Set the default namespace:
kubectl config set-context --current --namespace=SOME_NS

#Create two new deployments 
kubectl apply -f ..\..\helm\app1.yaml --namespace SOME_NS
kubectl apply -f ..\..\helm\app2.yaml --namespace SOME_NS

# Deploy INGRESS
kubectl apply -f ..\..\helm\internal-ingress.yaml --namespace SOME_NS

helm install nginx-ingress stable/nginx-ingress `
    --namespace SOME_NS `
    -f ..\..\helm\internal-ingress.yaml `
    --set controller.replicaCount=2 `
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux `
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux


# Deploy Tests apps, which should show up a default page at url
kubectl apply -f ..\..\helm\app1.yaml --namespace SOME_NS
kubectl apply -f ..\..\helm\app2.yaml --namespace SOME_NS

# Deploy the container for api
kubectl apply -f ..\..\helm\api.yaml --namespace SOME_NS
```

## 11. Initialize the Hashi Vault in a VM

The output of the shared terraform script should include the ip address of the pip of the vm.

```text
$pip_ipaddress = "SOME_IP"
ssh azureuser@$pip_ipaddress

# Run vault status command to check current status:
vault status

    Key                      Value
    ---                      -----
    Recovery Seal Type       azurekeyvault
    Initialized              false
    Sealed                   true
    Total Recovery Shares    0
    Threshold                0
    Unseal Progress          0/0
    Unseal Nonce             n/a
    Version                  n/a
    HA Enabled               true
    Notice that Initialized is false.

# Run the vault operator init command to initialize the Vault server so that you can unseal:

$regex = '[A-Za-z0-9+/]{44}|[a-z0-9]{8}-[a-z0-9]{4}-[a-z0-9]{4}-[a-z0-9]{4}-[a-z0-9]{12}'


$a = vault operator init

    Unseal Key 1: Kd.......................................4MC
    Unseal Key 2: pA.......................................FRN
    Unseal Key 3: YW.......................................eJj
    Unseal Key 4: +l......................................./XG
    Unseal Key 5: XK.......................................ex4

    Initial Root Token: UWN................Gw

    Vault initialized with 5 key shares and a key threshold of
    3. Please securely distribute the key shares printed above.
    When the Vault is re-sealed, restarted, or stopped, you 
    must supply at least 3 of these keys to unseal it before it
    can start servicing requests.

    Vault does not store the generated master key. Without at
    least 3 key to reconstruct the master key, Vault will 
    remain permanently sealed!

    It is possible to generate new unseal keys, provided you
    have a quorum of existing unseal keys shares. See “vault
    operator rekey” for more information


$a = kubectl exec -ti vault-0 -- vault operator init
$a -match $regex

$b = kubectl exec -ti vault-0 -- vault operator init -format "json"
```

## 12: Install default values into Vault

For this step, you'll need:

* The Following settings:
  * The IP Address of the Vault \(\)
  * The Secret of the Vault \(\)
  * The json blob of settings

An example json file might look like this:

```javascript
{
    "app1": {
      "Host": "localhost",
      "Port": 5000,
    },
    "ApplicationInsights": {
      "Disable": false,
    },
    "app2": {
      "Endpoint": "https://SOMEURL",
      "StatusEndpoint": "https://SOMEURL/status"
    }
}
```

Execute the following commands to upload the settings from a given app

