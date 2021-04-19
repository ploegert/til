# 7: Invoke Terraform



#### Execute Terraform

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
                                                                                                                                                                                  
Apply complete! Resources: 2 added, 0 changed, 1 destroyed.
```

