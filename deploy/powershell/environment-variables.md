# Environment Variables



Setup your environment variables:

#### Windows

```text
  # In this case, the subscription id = dv-dev-msdn and jci aad for tenant
  $env:ARM_CLIENT_ID = "SOME_ID"
  $env:ARM_CLIENT_SECRET = "SOME_PASSWORD"
  $env:ARM_SUBSCRIPTION_ID  = "SOME_GUID"
  $env:ARM_TENANT_ID  = "SOME_GUID"
```

#### Ubuntu

```bash
  export ARM_CLIENT_ID="SOME_ID"
  export ARM_CLIENT_SECRET="SOME_PASSWORD"
  export ARM_SUBSCRIPTION_ID="SOME_GUID"
  export ARM_TENANT_ID="SOME_GUID"
```

