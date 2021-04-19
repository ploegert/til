# Consul CLI Basics



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



### Export Consul Settings

```text
$v = consul kv export app/env/some.API/
$v | convertfrom-json | convertto-json | out-file "consul.json"
```



