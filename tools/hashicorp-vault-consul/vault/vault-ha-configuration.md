# Vault HA Configuration

### 1.Vault needs 3 vault servers to participate in the HA + its respective consul clusters as a back-end storage \(suggested 3 nodes\)

### 2.Once the required Redhat machines are spun up, we have install the enterprise binaries from provided S3

```text
sudo curl -O https://bootstrap.pypa.io/get-pip.py
sudo python get-pip.py
pip install awscli --upgrade --userdownload consul binaries:aws configure
AWS_ACCESS_KEY_ID=​​****
AWS_SECRET_ACCESS_KEY=****
us-east-2
jsonaws s3 cp s3://hc-enterprise-binaries/vault/prem/0.11.1/vault-enterprise_0.11.1+prem_linux_amd64.zip vault.zip
sudo unzip vault.zip
```

### 3. Once Vault is installed, place the respective vault config file as follows:

```text
storage "consul" {
    address = "localhost:8500"
    path = "vault"
    token = "consul-token"
    tls_skip_verify = 1
    tls_disable = 1
}

ui = true

listener "tcp" {
    address = "0.0.0.0:8200"
    cluster_address = "IP:8201"
    tls_disable = 1
}

api_addr = "http://IP:8200"
cluster_addr = "https://IP:8201"
```

### 4.To enable vault, we have to unseal and login with the root token.

### 5.To get the unseal keys and token, run 'vault operator init' command, and use any 3 keys to unseal the vault

