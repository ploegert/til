---
description: The following will export the vaultkey/values to a file.
---

# Export Vault Store

**Note: **Depends on GoLang

```csharp
[CmdletBinding()] 
Param(
    [string] $outpath = "d:\data",
    [string] $env = "charlie"
  )

BEGIN {
    
    # Setup Env Variables
    #$vault = @{url = $env:VAULT_ADDR; token=$env:VAULT_TOKEN }

    # $env:VAULT_ADDR = $vault.url
    # $env:VAULT_TOKEN = $vault.token

    #choco install golang
   
    # install the export tool
    write-host "Installing the golang utility to help export..."
    go get -u github.com/jsageryd/vault-kv-tool
    write-host "install Done." -foregroundcolor green
}


Process
{
    # Export secrets to a file
    $file = "$outpath/vault-temp.json"
    $outfile = "$outpath/vault.json"

    write-host "Do not cancel...this command may take a few hot minutes to complete...go get a snickers bar or something... "

    $var = "-root=DV/$env/"

    vault-kv-tool $var | jq . | out-file $file

    get-content $file | convertfrom-json | convertto-json -depth 10 | out-file $outfile

}

END {
    write-host "Done." -foregroundcolor green
}

```
