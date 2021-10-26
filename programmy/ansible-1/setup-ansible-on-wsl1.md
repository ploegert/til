# Setup Ansible on WSL1

We had a bit of a struggle to get ansible to work with all the dependencies - the following seemed to work for me

### Setup Ansible on WSL2

```bash
sudo apt-get update -y
sudo apt-get upgrade -y
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
sudo python3 ./get-pip.py
sudo python3 -m pip install ansible pywinrm
sudo python3 -m pip install ansible[azure]

export AZURE_SUBSCRIPTION_ID=SOMESECRET
export AZURE_CLIENT_ID=SOME_CLIENT
export AZURE_SECRET=SOME_SECRET
export AZURE_TENANT=SOME_TENANT

export VAULT_ADDR=SOME_URL
export VAULT_TOKEN=SOME_TOKEN

sudo ansible-galaxy collection install azure.azcollection --force
sudo python3 -m pip install -r ~/.ansible/collections/ansible_collections/azure/azcollection/requirements-azure.txt


pip install ansible-modules-hashivault
sudo python3 -m pip install ansible-modules-hashivault
sudo ansible-galaxy install 'git+https://github.com/TerryHowe/ansible-modules-hashivault.git'

cd /mnt/c/vso/OBT.Loki/deploy/ansible/
ansible-playbook infra_all.yml
```



Then i wanted to execute our first ansible script:

```bash
cd /mnt/c/vso/OBT.Loki/deploy/ansible/
ansible-playbook infra_all.yml
```

### Setup Azure Function Tools

```bash
wget -q https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
sudo apt-get install azure-functions-core-tools-3
```

