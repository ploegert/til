# Kubernetes secrets with Mozilla SOPS and Age encryption

There are various ways to manage secrets within a Kubernetes cluster but one of the easiest and most effective ways I manage secrets on my clusters is by using [Mozilla SOPS](https://github.com/getsops/sops) along with [age](https://github.com/FiloSottile/age) encryption.

I use Flux for GitOps on my clusters and so the cluster state is controlled by a GitHub repository. This repository is public facing and I wanted to include my secrets within the repository as well.

### SOPS <a href="#sops" id="sops"></a>

First, download the SOPS binary from the repository

Next, move the binary into a location of choice and update your OS environment variables with the path

#### Test CLI <a href="#test-cli" id="test-cli"></a>

Run `sops -v` to confirm it returns an output

### Age <a href="#age" id="age"></a>

First, download the Age binary from the repository

Next, move the binary into a location of choice and update your OS environment variables with the path

#### Test CLI <a href="#test-cli_1" id="test-cli_1"></a>

Run `age -version` to confirm it returns an output

### Generate Age key pair <a href="#generate-age-key-pair" id="generate-age-key-pair"></a>

```
age-keygen -o sops-age.key
```

### Add age config file to repository <a href="#add-age-config-file-to-repository" id="add-age-config-file-to-repository"></a>

To make encryption of secrets easier when working within the context of your GitOps repository you can create configuration files for automatic encryption. With this you can apply rules to look for particular data types as well as automatically use the correct age public key.

Normally you will use this for just secret data but I extend this to my ingress configuration as well to hide domain names and email addresses.

### Create .sops.yaml file <a href="#create-sopsyaml-file" id="create-sopsyaml-file"></a>

encrypt data and stringData types

```
creation_rules:  - path_regex: .*.yaml  -  encrypted_regex: '^(data|stringData)$'  - age: <age public key> 
```

encrypt email and dnsNames types

```
creation_rules:  - path_regex: .*.yaml  - encrypted_regex: '(email|dnsNames)'  - age: <age public key> 
```

### &#x20;<a href="#id-1" id="id-1"></a>

### Install Age key as secret <a href="#install-age-key-as-secret" id="install-age-key-as-secret"></a>

You will now need to install the Age generated key

### Working with Secrets <a href="#working-with-secrets" id="working-with-secrets"></a>

You are now ready to start encrypting secrets with



Source: [https://docs.binarybraids.com/containerisation/kubernetes/sops\_age/](https://docs.binarybraids.com/containerisation/kubernetes/sops_age/)
