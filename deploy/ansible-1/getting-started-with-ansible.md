# Getting started with Ansible

### Sample layout:

For Guidance: [https://docs.ansible.com/ansible/devel/user\_guide/sample\_setup.html](https://docs.ansible.com/ansible/devel/user_guide/sample_setup.html)

```text
production                # inventory file for production servers
staging                   # inventory file for staging environment

group_vars/
   group1.yml             # here we assign variables to particular groups
   group2.yml
host_vars/
   hostname1.yml          # here we assign variables to particular systems
   hostname2.yml

library/                  # if any custom modules, put them here (optional)
module_utils/             # if any custom module_utils to support modules, put them here (optional)
filter_plugins/           # if any custom filter plugins, put them here (optional)

site.yml                  # master playbook
webservers.yml            # playbook for webserver tier
dbservers.yml             # playbook for dbserver tier
tasks/                    # task files included from playbooks
    webservers-extra.yml  # <-- avoids confusing playbook with task files

roles/
    common/               # this hierarchy represents a "role"
        tasks/            #
            main.yml      #  <-- tasks file can include smaller files if warranted
        handlers/         #
            main.yml      #  <-- handlers file
        templates/        #  <-- files for use with the template resource
            ntp.conf.j2   #  <------- templates end in .j2
        files/            #
            bar.txt       #  <-- files for use with the copy resource
            foo.sh        #  <-- script files for use with the script resource
        vars/             #
            main.yml      #  <-- variables associated with this role
        defaults/         #
            main.yml      #  <-- default lower priority variables for this role
        meta/             #
            main.yml      #  <-- role dependencies
        library/          # roles can also include custom modules
        module_utils/     # roles can also include custom module_utils
        lookup_plugins/   # or other types of plugins, like lookup in this case

    webtier/              # same kind of structure as "common" was above, done for the webtier role
    monitoring/           # ""
    fooapp/               # ""
```

### Installing Azure Modules:

Using the Azure Resource Manager modules requires having specific Azure SDK modules installed on the host running Ansible.

```python
pip install ansible[azure]
```

If you are running Ansible from source, you can install the dependencies from the root directory of the Ansible repo.

```python
pip install .[azure]
```

### Using Environment Variables

To pass service principal credentials via the environment, define the following variables:

```text
AZURE_CLIENT_ID
AZURE_SECRET
AZURE_SUBSCRIPTION_ID
AZURE_TENANT
```

To pass Active Directory username/password via the environment, define the following variables:

```text
AZURE_AD_USER
AZURE_PASSWORD
AZURE_SUBSCRIPTION_ID
Storing in a File
```

When working in a development environment, it may be desirable to store credentials in a file. The modules will look for credentials in $HOME/.azure/credentials. This file is an ini style file. It will look as follows:

```text
[default]
subscription_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
client_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
secret=xxxxxxxxxxxxxxxxx
tenant=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
It is possible to store multiple sets of credentials within the credentials file by creating multiple sections. Each section is considered a profile. The modules look for the [default] profile automatically. Define AZURE_PROFILE in the environment or pass a profile parameter to specify a specific profile.
```

### Passing as Parameters

If you wish to pass credentials as parameters to a task, use the following parameters for service principal:

```text
client_id
secret
subscription_id
tenant
```

Or, pass the following parameters for Active Directory username/password:

```text
ad_user
password
subscription_id
```



### Create Credentials file:

Ansible looks in specific locations to auto load credentials if certain files exists. The Azure Ansible module uses the path ~/.azure/credentials. Placing a file in this location with the proper values will result in Ansible being able to connect to Azure. Keep in mind that credential files in Ansible are used for development environments. To use this method create a file at ~/.azure/credentials and populated the variables subscription\_id, client\_id, secret, and tenant.

#### Option 1: Create a credentials file

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

#### Populate the required Ansible variables. Replace  with actual values.

```text
[default]
subscription_id=<subscription_id>
client_id=<security-principal-appid>
secret=<security-principal-password>
tenant=<security-principal-tenant>
```

#### Option 2: use Env Variables

nstead of using a credentials file you can also populate specific environment variables that Ansible Azure module will use to connect to Azure. Using the bash command export you can define these values. Replace  with actual values.

```bash
export AZURE_SUBSCRIPTION_ID=<subscription_id>
export AZURE_CLIENT_ID=<security-principal-appid>
export AZURE_SECRET=<security-principal-password>
export AZURE_TENANT=<security-principal-tenant>
```

### Step 2 - Run an Ansible Playbook

After you provided the necessary values for Ansible to connect to Azure through either a credentials file or environment variables you can test the connection by running an Ansible playbook.

#### Create a playbook file

Ansible playbooks are written in YAML. Create a playbook by creating a new file named playbook.yaml and opening it in vi.

```bash
  vi playbook.yaml
```

#### Paste playbook contents in

Below is an Ansible playbook that creates an Azure resource group named rg-cs-ansible in the eastus region. It also registers the output to an Ansible variable and outputs it with the debug module. Copy and paste in the contents below to populate the playbook.

```yaml
---
- hosts: localhost
  connection: local
  tasks:
    - name: Create resource group
      azure_rm_resourcegroup:
        name: rg-cs-ansible
        location: eastus
      register: rg
    - debug:
        var: rg
```

#### Run the playbook using ansible-playbook

To execute the playbook use the ansible command ansible-playbook followed by the name of the playbook which is playbook.yaml. Once the playbook finishes running you will have a newly created resource group called rg-cs-ansible in Azure!

```bash
ansible-playbook playbook.yaml
```

## Example Creations

### Resource Group:

```yaml
- name: Create resource group
    azure_rm_resourcegroup:
    name: ansible-rg
    location: eastus
```

### Virtual network:

```yaml
- name: Create virtual network
    azure_rm_virtualnetwork:
    resource_group: ansible-rg
    name: vNet
    address_prefixes: "10.0.0.0/16"

- name: Add subnet
    azure_rm_subnet:
    resource_group: ansible-rg
    name: webSubnet
    address_prefix: "10.0.1.0/24"
    virtual_network: vNet
```

### Public IP:

```yaml
- name: Create public IP address
    azure_rm_publicipaddress:
    resource_group: ansible-rg
    allocation_method: Static
    name: webPublicIP
    register: output_ip_address

- name: Output public IP
    debug:
    msg: "The public IP is {{ output_ip_address.state.ip_address }}"
```

### NSG

```yaml
- name: Create Network Security Group
    azure_rm_securitygroup:
    resource_group: ansible-rg
    name: networkSecurityGroup
    rules:
        - name: 'allow_rdp'
        protocol: Tcp
        destination_port_range: 3389
        access: Allow
        priority: 1001
        direction: Inbound
        - name: 'allow_web_traffic'
        protocol: Tcp
        destination_port_range:
            - 80
            - 443
        access: Allow
        priority: 1002
        direction: Inbound
        - name: 'allow_powershell_remoting'
        protocol: Tcp
        destination_port_range: 
            - 5985
            - 5986
```

### Full file for a VM:

```yaml
#deployWindowsAzureVirtualMachine.yaml
---
- hosts: localhost
  connection: local

  vars_prompt:
    - name: password
      prompt: "Enter local administrator password"

  tasks:
    - name: Create resource group
      azure_rm_resourcegroup:
        name: ansible-rg
        location: eastus

    - name: Create virtual network
      azure_rm_virtualnetwork:
        resource_group: ansible-rg
        name: vNet
        address_prefixes: "10.0.0.0/16"

    - name: Add subnet
      azure_rm_subnet:
        resource_group: ansible-rg
        name: webSubnet
        address_prefix: "10.0.1.0/24"
        virtual_network: vNet

    - name: Create public IP address
      azure_rm_publicipaddress:
        resource_group: ansible-rg
        allocation_method: Static
        name: webPublicIP
      register: output_ip_address

    - name: Output public IP
      debug:
        msg: "The public IP is {{ output_ip_address.state.ip_address }}"

    - name: Create Network Security Group
      azure_rm_securitygroup:
        resource_group: ansible-rg
        name: networkSecurityGroup
        rules:
          - name: 'allow_rdp'
            protocol: Tcp
            destination_port_range: 3389
            access: Allow
            priority: 1001
            direction: Inbound
          - name: 'allow_web_traffic'
            protocol: Tcp
            destination_port_range:
              - 80
              - 443
            access: Allow
            priority: 1002
            direction: Inbound
          - name: 'allow_powershell_remoting'
            protocol: Tcp
            destination_port_range: 
              - 5985
              - 5986
            access: Allow
            priority: 1003
            direction: Inbound

    - name: Create a network interface
      azure_rm_networkinterface:
        name: webNic
        resource_group: ansible-rg
        virtual_network: vNet
        subnet_name: webSubnet
        security_group: networkSecurityGroup
        ip_configurations:
          - name: default
            public_ip_address_name: webPublicIP
            primary: True


    - name: Create VM
      azure_rm_virtualmachine:
        resource_group: ansible-rg
        name: winWeb01
        vm_size: Standard_DS1_v2
        admin_username: azureuser
        admin_password: "{{ password }}"
        network_interfaces: webNic
        os_type: Windows
        image:
          offer: WindowsServer
          publisher: MicrosoftWindowsServer
          sku: 2019-Datacenter
          version: latest
```

### Get a Fact:

```yaml
   - name: Get facts for one Public IP
      azure_rm_publicipaddress_info:
        resource_group: ansible-rg
        name: webPublicIP
      register: publicipaddresses
```

### Waiting for a resource:

```yaml
    - name: wait for the WinRM port to come online
      wait_for:
        port: 5986
        host: '{{ publicipaddress }}'
        timeout: 600
```

## Resources:

* [Connecting to Azure with Ansible](https://dev.to/cloudskills/connecting-to-azure-with-ansible-22g2)
* Read more about [Providing Credentials to Azure Modules](https://docs.ansible.com/ansible/latest/scenario_guides/guide_azure.html#providing-credentials-to-azure-modules).
* [Quickstart: Install Ansible on Linux virtual machines in Azure](https://docs.microsoft.com/en-us/azure/ansible/ansible-overview)
* [Create an Azure service principal with Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/create-azure-service-principal-azureps?view=azps-3.1.0)
* [New-AzRoleAssignment](https://docs.microsoft.com/en-us/powershell/module/az.resources/new-azroleassignment?view=azps-3.1.0)
* [Cloudskills.io](https://cloudskills.io/blog/connect-to-azure-with-ansible)
* [Best Practices](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html)

