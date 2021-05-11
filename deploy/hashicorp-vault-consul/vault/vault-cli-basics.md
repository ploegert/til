# Vault CLI Basics

## Setup Env Variables

```text
# Setup Env Variables
$vault = @{url = "https://127.0.0.1:8200"; token="secure string" }
$consul = @{url = "http://127.0.0.1:8500"; token="secure string" }

$env:VAULT_ADDR = $vault.url
$env:VAULT_TOKEN = $vault.token  

$env:CONSUL_HTTP_ADDR = $consul.url
$env:CONSUL_HTTP_TOKEN = $consul.token
```

## TEST a new cluster:



```text
  vault login -address="https://URL:8200" "SOMEPASS"
  vault login -address=$vault.url $vault.token


  vault secrets enable -version=1 -path DV kv
  vault kv put app/env/"some.API"/1.0/Redis TLS=False
```

## Show we can connect to secrets

```text
  # Show we can connect to secrets
  vault secrets list

  # show secrets example
  vault kv get app/env/some.API/1.0/ApplicationInsights/InstrumentationKey
```

## Export Vault Settings

[Source of Exporting Vault](https://github.com/jsageryd/vault-kv-tool)

**Bash**

```text
  sudo add-apt-repository ppa:longsleep/golang-backports
  sudo apt update
  sudo apt install golang-go
  go version
  export GOPATH=~/go
  export PATH=$PATH:$GOPATH/bin

  # install the export tool
  go get -u github.com/jsageryd/vault-kv-tool

  export VAULT_ADDR=http://localhost:8200/
  export VAULT_TOKEN=SOMEPASS

  # Export secrets to a file
  vault-kv-tool -root=env/ > data.json
```

Pwsh:

```text
  # install the export tool
  go get -u github.com/jsageryd/vault-kv-tool

  # Export secrets to a file
  $file = "secrets-vault-.json"
  vault-kv-tool -root=app/env/'some.API'/ | out-file $file
```

## Import Vault Settings

```text
# Import Vault file
  vault secrets enable -version=1 -path DV kv
  get-content | jq . | vault-kv-tool -root=app/env/'some.API'/ -write

  # Alternative using the client itself:
  vault kv put app/env/some.API/1.0 @kyron-api.json
  vault kv put app/env/some.API/1.0 @kyron-api.json --format
  
  $path = "app/env/some.API/1.0"
  $server = "https://URL8200"
  $token = "SOMEPASS"
  $headers = @{"X-Vault-Token"=$token}
  $HTTPServer = "$server/v1/$path"

  vault write -format=json $path @some-api.json

  curl --header "X-Vault-Token: SOMEPASS" --request POST --data @some-api.json https://URL:8200/v1/app/env/some.API/1.0



Invoke-WebRequest -H $headers -UseBasicParsing -method POST -body @some-api.json -Uri "$server/secret/data/$path"
Invoke-RestMethod -Headers $headers -Method Post $HTTPServer 

```

