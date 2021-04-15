# Getting Started with Terraform

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

## 

## 4: Setup your Workspace

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

## 5: Run the initial Terraform deploy of shared stuffs:

```text
  $basepath = "c:/vso"
  set-location $basepath/OBT.Deploy/terraform/base-env-setup

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
  * Traffic Mangager \(For DNS name\)
  * Log Analytics

## 6. Run the Terraform DB Schema

Now we will need to manually add the firewall rules to allow the aks cluster to reach the pgsql.

1. Open the [https://portal.azure.com](https://portal.azure.com), and select that resource group for obtdz1-core-rg.
2. Select the obtdz1-core-hyperscale server group
3. Then select the Networking from the Settings category on the bottom left
4. There is an item that says "Allow Azure services and resources to access this server group". Change this selection from no to YES
5. You'll need to also add the public ip of your machine to the firewall rule, otherwise your schema scripts will fail. you can find out your ip address by typing "whats my ip" in google search.

Now that we have configured the access firewalls, we can configure run the schema scripts.

```text
set-location $basepath/terraform/kyron.db

  # Verify that you'vev downloaded all the terraform provider
  terraform init

  # Verify that everything builds
  terraform validate

  # Make it so!
  terraform apply
```

## 7: Export the k8s credentials:

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

## 8: Generate Secrets used for ingress

```text
# Generate new self signed cert to use for TLS within the cluster
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout "./certs/ingress.key" -out "./certs/ingress.pem" -subj "/CN=ingresstls/O=JohnsonControls"

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

## 8: Deploy the Kyron API

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

## 6. Initialize the Hashi Vault in a VM

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

## 7: Install default values into Vault

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

