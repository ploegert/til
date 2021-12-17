---
description: This page will show you how to export all keys
---

# Azure Keyvault



```csharp
[CmdletBinding()] 
Param(
    [hashtable]$dev3 = @{name="SUB_NAME";id="SUB_ID"},
    [string] $env = "c",
    [string] $app = "app",
    [string] $zone = "zone1"
    
  )

BEGIN {

    function Export-AllKeys
    {
        [cmdletbinding()]
        Param(
            $subscription_id, 
            $kv_name,
            $out_file = "$kv_name.json")
    
        $splat = @(
            "--vault-name", $kv_name
            "--subscription", "$subscription_id")
    
        $keyVaultEntries = (az keyvault secret list @splat | ConvertFrom-Json) | Select-Object id, name
    
        Write-Host "Secret values of '$($subscription_id)' for key vault '$($kv_name)'"
        $Key_List = [System.Collections.ArrayList]@()
        foreach($entry in $keyVaultEntries)
        {
            write-host "Processing $($entry.name)..."
            $secretValue = (az keyvault secret show --id $entry.id | ConvertFrom-Json) | Select-Object name, value
   
            $item = [PSCustomObject]@{
                name  = $secretValue.name
                value = $secretValue.value
                id    = $entry.id
            }
            $Key_List.add($item)
    
            $filename = "keys/$($entry.name).txt"
            $null > $filename
            az keyvault secret backup --file $filename --name $entry.name @splat

            write-host "done." -ForegroundColor green
        }
        Write-Host ""
        
        \write-host "Saving keys to file: $out_file"
        $Key_List | convertto-json | out-file "$out_file"
    }

}

Process
{

    $prefix = "${app}${env}${zone}"
    $subscription_id = $dev3.id

    ## Log in to Azure
    #az login
    ## Set your subscription
    az account set --subscription $subscription_id

    ## Register Key Vault as a provider
    az provider register -n Microsoft.KeyVault

    $kv_name = "${prefix}-loki-kv"

    Export-AllKeys -subscription_id $subscription_id -kv_name $kv_name
}

END {


}

# ## Back up a certificate in Key Vault
# az keyvault certificate backup --file {File Path} --name {Certificate Name} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

# ## Back up a key in Key Vault
# az keyvault key backup --file {File Path} --name {Key Name} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

# ## Back up a secret in Key Vault
# az keyvault secret backup --file {File Path} --name {Secret Name} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

# ## Restore a certificate in Key Vault
# az keyvault certificate restore --file {File Path} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

# ## Restore a key in Key Vault
# az keyvault key restore --file {File Path} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

# ## Restore a secret in Key Vault
# az keyvault secret restore --file {File Path} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

```
