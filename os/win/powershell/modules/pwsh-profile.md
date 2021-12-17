# Pwsh Profile

I like to setup my powershell profile to load my settinsg for:

* ARM Client
* Consul/Vault
* Docker Repo

In this way, it will help me with connecting to the relevant things, and mimics variables we load in build agents

```csharp
Import-Module posh-git
Import-Module oh-my-posh
Set-Theme Paradox

# Chocolatey profile
$ChocolateyProfile = "$env:ChocolateyInstall\helpers\chocolateyProfile.psm1"
if (Test-Path($ChocolateyProfile)) {
  Import-Module "$ChocolateyProfile"
}

write-host "ohai!"

write-host "Loading ARM Client, Secret, Sub, Tenant." -ForegroundColor yellow
$env:ARM_CLIENT_ID                        = "REDACTED"
$env:ARM_CLIENT_SECRET                    = "REDACTED"
$env:ARM_SUBSCRIPTION_ID                  = "REDACTED"
$env:ARM_TENANT_ID                        = "REDACTED"
write-host "done." -ForegroundColor green

write-host "Loading vault/consul settings..." -ForegroundColor yellow
$env:VAULT_ADDR                           = "REDACTED"
$env:VAULT_TOKEN                          = "REDACTED"
$env:CONSUL_HTTP_ADDR                     = "REDACTED"
$env:CONSUL_HTTP_TOKEN                    = "REDACTED"
write-host "done." -ForegroundColor green

$env:DOCKER_REPO_URL                      = "REDACTED"
$env:DOCKER_REPO_USER                     = "REDACTED"
$env:DOCKER_REPO_PASS                     = "REDACTED"
```



