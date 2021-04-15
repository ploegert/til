# 2: Setup Env Variables

## 2: Setup your environment variables:

In order to use terraform in a secure way, the spn id/secrets should not be stored in source code, so each time you want to setup an env, you'll want to change your env variables to the necessary accounts that have access. Here is an example way of using powershell to create the evn settings

#### Windows

```text
  # In this case, the subscription id = dv-dev-msdn and jci aad for tenant
  $env:ARM_CLIENT_ID = "SOME_ID"
  $env:ARM_CLIENT_SECRET = "SOME_PASSWORD"
  $env:ARM_SUBSCRIPTION_ID  = "SOME_SUBSCRIPTION"
  $env:ARM_TENANT_ID  = "SOME_AAD_TENANT"
```

#### Ubuntu

```bash
  export ARM_CLIENT_ID="SOME_ID"
  export ARM_CLIENT_SECRET="SOME_PASSWORD"
  export ARM_SUBSCRIPTION_ID="SOME_SUBSCRIPTION"
  export ARM_TENANT_ID="SOME_AAD_TENANT""
```

## 

