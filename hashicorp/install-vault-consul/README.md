# Install Vault/Consul

\*\*\*\*

### Resources

* Vault
  * [Run Vault on Kubernetes](https://www.vaultproject.io/docs/platform/k8s/helm/run)
  * [Announcing hashi vault helm chart](https://www.hashicorp.com/blog/announcing-the-vault-helm-chart/)
  * [github vault-helm charts from Hashi](https://github.com/hashicorp/vault-helm)
  * Seal Vs Unseal:
    * [Concepts: Seal vs Unseal](https://www.vaultproject.io/docs/concepts/seal)
    * [auto-Unseal using Azure Key Vault](https://learn.hashicorp.com/tutorials/vault/autounseal-azure-keyvault)
    * [auto-unseal example scripts using terraform](https://github.com/hashicorp/vault-guides/blob/master/operations/azure-keyvault-unseal/main.tf)
    * [Terraform azurekeyvault provider](https://www.vaultproject.io/docs/configuration/seal/azurekeyvault)
    * From Lava - [Auto-unseal using transit secrets engine](https://learn.hashicorp.com/tutorials/vault/autounseal-transit?in=vault/auto-unseal)
* Consul
  * [Installing Consul on K8s](https://www.consul.io/docs/k8s/installation/overview)
  * [Consul helm chart reference](https://www.consul.io/docs/k8s/helm)
* Exporting Values:
  * [Vault-Export kv tool - Not officially supported](https://github.com/jsageryd/vault-kv-tool)
  * [Consul-export docs](https://www.consul.io/docs/commands/kv/export)
  * [Consul-import docs](https://www.consul.io/docs/commands/kv/import)
  * [Consul-CLI reference](https://www.consul.io/docs/commands)
  * [First Look at kv store in consul](http://alesnosek.com/blog/2017/07/15/first-look-at-the-key-value-store-in-consul/)
* [https://www.hashicorp.com/blog/announcing-the-vault-helm-chart/](https://www.hashicorp.com/blog/announcing-the-vault-helm-chart/)
* [https://github.com/hashicorp/vault-helm](https://github.com/hashicorp/vault-helm)
* [https://www.vaultproject.io/docs/platform/k8s](https://www.vaultproject.io/docs/platform/k8s)
* [https://www.vaultproject.io/docs/platform/k8s/helm/run](https://www.vaultproject.io/docs/platform/k8s/helm/run)

**On Windows**

I like [chococately](https://chocolatey.org/), so you can install using:

```text
# install choco
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

# install vault
choco install vault -y

choco install consul -y
```

**On Ubuntu**

```bash
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
```

### Install consul in k8s

```text
# set default Namespace context
kubectl config set-context --current --namespace=app-d

# Add Repo
helm repo add hashicorp https://helm.releases.hashicorp.com
helm install consul hashicorp/consul --set global.name=consul --namespace obt-d

helm search repo hashicorp/consul
helm status consul
helm get all consul
```

### Install Vault in k8s

```text
# Add Repo
helm repo add hashicorp https://helm.releases.hashicorp.com
helm search repo hashicorp/vault

# Install the chart
helm install vault hashicorp/vault --set global.name=vault --namespace app-d
helm status vault
helm get all vault

# Check status
kubectl get pods -l app.kubernetes.io/name=vault

kubectl exec -it vault-0 -- vault status

# Initialize
$regex = '[A-Za-z0-9+/]{44}|[a-z0-9]{8}-[a-z0-9]{4}-[a-z0-9]{4}-[a-z0-9]{4}-[a-z0-9]{12}'
$a = kubectl exec -ti vault-0 -- vault operator init
$a -match $regex

$b = kubectl exec -ti vault-0 -- vault operator init -format "json"


"{{ vault_init_output.stdout | regex_findall ('(?<=Initial Root Token:\\s).*$', multiline=True, ignorecase=True) }}"

$a -match '(?<=Initial Root Token:\\s).*$'
$a -match '(?<=Initial Root Token: ).*$'
$a -replace '.*Initial Root Token:.(.*)$', '\\1'

$a -match 'Initial Root Token: ([^\n\r]*)'

$r1 = 'Initial Root Token: ([^\n\r]*)'
$k1
$k2
$k3
$k4
$k5


kubectl exec -ti vault-0 -- vault operator init | grep -Po '[A-Za-z0-9+/]{44}|[a-z0-9]{8}-[a-z0-9]{4}-[a-z0-9]{4}-[a-z0-9]{4}-[a-z0-9]{12}'


kubectl exec -ti vault-0 -- vault operator init | Select-String -Pattern '[A-Za-z0-9+/]{44}|[a-z0-9]{8}-[a-z0-9]{4}-[a-z0-9]{4}-[a-z0-9]{4}-[a-z0-9]{12}'

kubectl exec -ti vault-0 -- vault operator init
kubectl exec -it vault-0 -- vault operator init -n 1 -t 1

# Unseal vault
kubectl exec -it vault-0 -- vault operator unseal <unsealkey>

kubectl exec -it vault-0 -- vault operator unseal 7f......WOX
kubectl exec -it vault-0 -- vault operator unseal W8......PoC
kubectl exec -it vault-0 -- vault operator unseal ao......zED
kubectl exec -it vault-0 -- vault operator unseal 5t......I0e
kubectl exec -it vault-0 -- vault operator unseal Oa......0iy

Initial Root Token: s.fm.....J4VJNF0

# Alternative - install the chart in dev mode
helm install --name=vault --set='server.dev.enabled=true' .

# Port forwarding
kubectl port-forward vault-0 8200:8200

# View all the Vault pods in the current namespace:
kubectl get pods -l app.kubernetes.io/name=vault

# Initialize one Vault server with the default number of key shares and default key threshold:
kubectl exec -ti vault-0 -- vault operator init


# Repeat the unseal process for all Vault server pods. When all Vault server pods are unsealed they report READY 1/1.
kubectl get pods -l app.kubernetes.io/name=vault
```



```text
vault status
vault operator init
sudo systemctl restart vault
vault status
cat /etc/vault.d/config.hcl
vault login SOMEPASS
cat /tmp/azure_auth.sh
./tmp/azure_auth.sh
ls
cd /tmp
./azure_auth.sh
sudo journalctl --no-pager -u vault

vault token create
history
```

