# 4: Deployment Agent Setup

## Setting up Terraform

### Instructions to setup terraform

Terraform can be used executed locally using your own env, in a cloud shell via azure, or even via a docker image. This explanation will go through setting up natively on your own posh shell.

Dependencies:

* Terraform
* Azure CLI
* psql
* Kubectl
* helm
* consul
* vault
* ansible
* pip

#### Installation of Terraform & Azure CLI

Make sure you install terraform from here: [https://www.terraform.io/downloads.html](https://www.terraform.io/downloads.html).

**On Windows**

I like [chococately](https://chocolatey.org/), so you can install using:

```text
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

### Setup Ansible on WSL2

```bash
sudo apt-get update -y
sudo apt-get upgrade -y
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
sudo python3 ./get-pip.py
sudo python3 -m pip install ansible pywinrm
sudo python3 -m pip install ansible[azure]
export AZURE_SUBSCRIPTION_ID=SOME_SUBSCRIPTION
export AZURE_CLIENT_ID=SOME_CLIENT_ID
export AZURE_SECRET=SOME_CLIENT_SECRET
export AZURE_TENANT=SOME_AAD_CLIENT

sudo ansible-galaxy collection install azure.azcollection --force
sudo python3 -m pip install -r ~/.ansible/collections/ansible_collections/azure/azcollection/requirements-azure.txt
cd /mnt/c/vso//deploy/ansible/
ansible-playbook infra_all.yml
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

