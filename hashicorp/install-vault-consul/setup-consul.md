# Setup Consul

## Install Consul:

[https://learn.hashicorp.com/tutorials/consul/deployment-guide](https://learn.hashicorp.com/tutorials/consul/deployment-guide)

```bash
export CONSUL_VERSION="1.8.3"
export CONSUL_URL="https://releases.hashicorp.com/consul"

curl --silent --remote-name ${CONSUL_URL}/${CONSUL_VERSION}/consul_${CONSUL_VERSION}_linux_amd64.zip
curl --silent --remote-name ${CONSUL_URL}/${CONSUL_VERSION}/consul_${CONSUL_VERSION}_SHA256SUMS
curl --silent --remote-name ${CONSUL_URL}/${CONSUL_VERSION}/consul_${CONSUL_VERSION}_SHA256SUMS.sig

sudo apt-get install zip -y

unzip consul_${CONSUL_VERSION}_linux_amd64.zip

sudo chown root:root consul
sudo mv consul /usr/bin/
consul --version
consul -autocomplete-install
complete -C /usr/bin/consul consul
sudo useradd --system --home /etc/consul.d --shell /bin/false consul
sudo mkdir --parents /opt/consul
sudo chown --recursive consul:consul /opt/consul

consul


# Generate gossip encryption key
consul keygen

# craete CA
consul tls ca create
        ==> Saved consul-agent-ca.pem
        ==> Saved consul-agent-ca-key.pem

# Create tls certs
consul tls cert create -server -dc <dc_name>
consul tls cert create -server -dc dc1

        ==> WARNING: Server Certificates grants authority to become a
            server and access all state in the cluster including root keys
            and all ACL tokens. Do not distribute them to production hosts
            that are not server nodes. Store them as securely as CA keys.
        ==> Using consul-agent-ca.pem and consul-agent-ca-key.pem
        ==> Saved dc1-server-consul-0.pem
        ==> Saved dc1-server-consul-0-key.pem

# Create Client Creds:
 consul tls cert create -client -dc <dc_name>
 consul tls cert create -client -dc dc1
        ==> Using consul-agent-ca.pem and consul-agent-ca-key.pem
        ==> Saved dc1-client-consul-0.pem
        ==> Saved dc1-client-consul-0-key.pem
```

