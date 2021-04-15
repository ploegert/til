# Configure Ingress

### Convert a PFX to a Base64 string:

```text
$fileContentBytes = get-content 'bravo.someurl.com.pfx' -Encoding Byte
```

### Generate a self signed cert

```text
#openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout ${KEY_FILE} -out ${CERT_FILE} -subj "/CN=${HOST}/O=${HOST}"

$path="C:\Users\justi\Desktop\dev_someurl.com\ingress-certs"
$keyFile="$path\ingress.key"
$certFile="$path\ingress.pem"
$cnname="ingress-ssl"


$ag = @{
    days=365;
    newKey="rsa:2048"
    keyout=$keyFile
    Out=$certFile
    subject="/CN=${cnname}/O=${cnname}"
}

openssl req -x509 -nodes @ag
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout "./ingress.key" -out "./ingress.pem" -subj "/CN=${HOST}/O=${HOST}"

$key = get-content ./ingress.key
$pem = get-content ./ingress.pem

$namespace = "app-d"
$secretname = "ingresstls"

kubectl create secret tls $secretname `
    --namespace $namespace `
    --key $key `
    --cert $pem

kubectl create secret tls $secretname `
    --namespace $namespace `
    --key "./ingress.key" `
    --cert "./ingress.pem"


kubectl create secret docker-registry $ACR_SHORTNAME `
  --docker-server=$ACR_NAME `
  --docker-username=$ACR_UNAME `
  --docker-password=$ACR_PASSWD `
  --docker-email=SOME_EMAIL
```

[https://github.com/hashicorp/vault-guides/blob/master/operations/provision-vault/best-practices/terraform-aws/main.tf](https://github.com/hashicorp/vault-guides/blob/master/operations/provision-vault/best-practices/terraform-aws/main.tf)

#### Here is another way of generating the certs using modules

```text
module "root_tls_self_signed_ca" {
   source = "github.com/hashicorp-modules/tls-self-signed-cert"

  name              = "${var.name}-root"
  ca_common_name    = "${var.common_name}"
  organization_name = "${var.organization_name}"
  common_name       = "${var.common_name}"
  download_certs    = "${var.download_certs}"

  validity_period_hours = "8760"

  ca_allowed_uses = [
    "cert_signing",
    "key_encipherment",
    "digital_signature",
    "server_auth",
    "client_auth",
  ]
}

module "leaf_tls_self_signed_cert" {
  source = "github.com/hashicorp-modules/tls-self-signed-cert"

  name              = "${var.name}-leaf"
  organization_name = "${var.organization_name}"
  common_name       = "${var.common_name}"
  ca_override       = true
  ca_key_override   = "${module.root_tls_self_signed_ca.ca_private_key_pem}"
  ca_cert_override  = "${module.root_tls_self_signed_ca.ca_cert_pem}"
  download_certs    = "${var.download_certs}"

  validity_period_hours = "8760"

  dns_names = [
    "localhost",
    "*.node.consul",
    "*.service.consul",
    "server.dc1.consul",
    "*.dc1.consul",
    "server.${var.name}.consul",
    "*.${var.name}.consul",
  ]

  ip_addresses = [
    "0.0.0.0",
    "127.0.0.1",
  ]

  allowed_uses = [
    "key_encipherment",
    "digital_signature",
    "server_auth",
    "client_auth",
  ]
}
```

