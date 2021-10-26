# Vault Unseal



### Execute the following command to retrieve your virtual machine information:

```text
terraform output ssh-addr
```

### SSH to connect to your virtual machine with username, azureuser:

```text
ssh azureuser@<IP_address>
```

### Run vault status command to check current status:

```text
vault status

Key                      Value
---                      -----
Recovery Seal Type       azurekeyvault
Initialized              false
Sealed                   true
Total Recovery Shares    0
Threshold                0
Unseal Progress          0/0
Unseal Nonce             n/a
Version                  n/a
HA Enabled               true
Notice that Initialized is false.
```

### Run the vault operator init command to initialize the Vault server so that you can unseal:

```text
vault operator init
```

### Check the Vault status to verify that it has been initialized and unsealed.

```text
vault status

        Key                      Value
        ---                      -----
        Recovery Seal Type       shamir
        Initialized              true
        Sealed                   false
        Total Recovery Shares    5
        Threshold                3
        Version                  1.3.0
        Cluster Name             vault-cluster-092ba5de
        Cluster ID               8b173565-7d74-fe5b-a199-a2b56b7019ee
        HA Enabled               false
        Notice that the Vault server is already unsealed (Sealed is false).
```

### In the service log, you should find a trace where Azure Vault key is being fetched to unseal the Vault server.

```text
sudo journalctl --no-pager -u vault

        ...
        ==> Vault server configuration:
                Azure Environment: AzurePublicCloud
                Azure Key Name: generated-key
                Azure Vault Name: Test-vault-a414d041
                        Seal Type: azurekeyvault
                            Cgo: disabled
                    Listener 1: tcp (addr: "0.0.0.0:8200", cluster address: "0.0.0.0:8201", max_request_duration: "1m30s", max_request_size: "33554432", tls: "disabled")
                        Log Level: (not set)
                            Mlock: supported: true, enabled: false
                        Storage: file
                        Version: Vault v1.3.0
                    Version Sha: 37a1dc9c477c1c68c022d2084550f25bf20cac33
        ==> Vault server started! Log data will stream in below:
        2019-01-22T19:43:29.114Z [WARN]  no `api_addr` value specified in config or in VAULT_API_ADDR; falling back to detection if possible, but this value should be manually set
        2019-01-22T19:43:29.121Z [INFO]  core: stored unseal keys supported, attempting fetch
        2019-01-22T19:43:29.162Z [INFO]  core: vault is unsealed
        ...

```

### Restart the Vault server to ensure that Vault server gets automatically unsealed:

```text
sudo systemctl restart vault
vault status
```

### Explorer the systemd configuration for Vault server which is located at /lib/systemd/system/vault.service:

```text
cat /lib/systemd/system/vault.service

        [Unit]
        Description=Vault Agent
        Requires=network-online.target
        After=network-online.target
        [Service]
        Restart=on-failure
        PermissionsStartOnly=true
        ExecStartPre=/sbin/setcap 'cap_ipc_lock=+ep' /usr/local/bin/vault
        ExecStart=/usr/local/bin/vault server -config /etc/vault.d/config.hcl
        ExecReload=/bin/kill -HUP
        KillSignal=SIGTERM
        User=azureuser
        Group=azureuser
        [Install]
        WantedBy=multi-user.target
        Review the Vault configuration file (/etc/vault.d/config.hcl):

cat /etc/vault.d/config.hcl

        storage "file" {
        path = "/opt/vault"
        }
        listener "tcp" {
        address     = "0.0.0.0:8200"
        tls_disable = 1
        }
        seal "azurekeyvault" {
        client_id      = "YOUR-APP-ID"
        client_secret  = "YOUR-APP-PASSWORD"
        tenant_id      = "YOUR-AZURE-TENANT-ID"
        vault_name     = "Test-vault-XXXXXX"
        key_name       = "generated-key"
        }
        ui=true
        disable_mlock = true
```

