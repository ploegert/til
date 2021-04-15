# 5: Setting up remote state for Terraform

#### Setting up remote state for Terraform

In order to save state to a remote storage location \(azure blob\), we'll need to setup a storage accound and some containers. Y ou'll do that by executing the terraform script under /terraform/1-tf-remotestate.

NOTE: If this has already been executed, you don't need to re-run this 1-tf-remotestate. This exists for the new obtdz1 env!

```text
# Put yourself in the root of the terraform folder (not the module level - the root l evel)
set-location "./terraform/1-tf-remotestate"

# Type terraform validate to validate the config
terraform validate

#Output
Success! The configuration is valid.
```

