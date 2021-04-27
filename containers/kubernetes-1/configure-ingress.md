# Configure Ingress

## Preparing local Linux environment or VM.

Do the following once to prepare your environment:

Reference: [https://taasjci.atlassian.net/wiki/spaces/GS/pages/900531038/Creating+Ingress+controller+-+Completed](https://taasjci.atlassian.net/wiki/spaces/GS/pages/900531038/Creating+Ingress+controller+-+Completed)

* If you are deploying from Win10 laptop, enable Linux Subsystem on Windows Features and install Linux subsystem of your choice. The document below assumes that Ubuntu 16.x was installed. Follow [https://docs.microsoft.com/en-us/windows/wsl/install-win10](https://docs.microsoft.com/en-us/windows/wsl/install-win10) for reference if necessary. As an alternative, any Linux machine or VM could be used.
* Install Helm client from [https://github.com/helm/helm/releases](https://github.com/helm/helm/releases) \(this document was tested with client version 2.9.0, [https://storage.googleapis.com/kubernetes-helm/helm-v2.9.0-linux-amd64.tar.gz](https://storage.googleapis.com/kubernetes-helm/helm-v2.9.0-linux-amd64.tar.gz)\).
* AKS only: use az aks install-cli to install kubectl client. Refer MS documentation for details.
* OpenShift only: Install OpenShift client from [https://www.okd.io/download.html](https://www.okd.io/download.html) \(this document was tested with client version 3.11.0, [https://github.com/openshift/origin/releases/download/v3.11.0/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz](https://github.com/openshift/origin/releases/download/v3.11.0/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz)\).
* Ensure that helm, kubectl \(and oc if OpenShift is used\) binaries are included in your PATH. On Win10-based Linux subsystem you can just move these binaries to ~/bin. Note: oc binary is full copy of kubectl binary if OpenShift is used.
* Ensure that primary nameserver in /etc/resolv.conf is 172.30.1.135. Note that Win10-based Linux subsystem uses to prepend 8.8.8.8 and 8.8.4.4 nameservers as primary.
* AKS only: use az aks get-credentials -f .... to update kubectl configuration with credentials of your server. Refer MS documentation for details.
* OpenShift only: Login with oc CLI tool. The easiest way to get login credentials for CLI login is to login to OpenShift console in your browser, click on your name at top right corner and copy login command. It will be looking like "oc login [https://be-deb-mgmt.debinternal.cloud:443](https://be-deb-mgmt.debinternal.cloud:443) --token=5lD3gNwU..............". Run this command in your Linux shell and ensure that it works and you see tiller namespace in the list of available namespaces.

## Helm Setup

For deployment of Ingress controller, stock template from [https://github.com/helm/charts/tree/master/stable/nginx-ingress](https://github.com/helm/charts/tree/master/stable/nginx-ingress) project is used with some customization.

Clone this project with git:

```text
git clone https://github.com/helm/charts.git
```

Under stable/nginx-express/templates/, edit controller-deployment.yaml and remove the following block lines 104 to 112:

```text
104     securityContext:
105            Capabi8lities:
106                drop:
107                    - All
108                add:
109                   - NET_BIND_SERVICE
110            runAsUser: {{ ,..}}
111            allowPrivilegeEscalation {{ ... }}
112        {{- end }}
113        env
```

Under stable/nginx-express/, edit values.yaml and set controller.name and ingressClass to match your environment, like below:

```yaml
controller:
    name: as-dev
    image: 
        repository: quay.io/kubernets-ing...
        tag: ....
```

```yaml
ingresClass: as-dev
```

### Create Kubernetes secret:

kubectl create secret tls aks-ingress-tls --namespace ingress-basic --key aks-ingress-tls.key --cert aks-ingress-tls.crt

The files aks-ingress-tls.key and aks-ingress-tls.crt are  and they are stored in lastpass

### Create a policy for tiller:

kubectl policy add-role-to-user edit "system:serviceaccount:${TILLER\_NAMESPACE}:tiller"

### Install Helm Chart

Install helm chart \(substitute your nginx container name and namespace name\):

helm install --name nginx-as-dev nginx-ingress --namespace as-dev

### Check Ingres

Check that nginx-ingress container was started \(substitute your namespace name\):

kubectl get services –-namespace as-dev

## Configuring Certs

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

