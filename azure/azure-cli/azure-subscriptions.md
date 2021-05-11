# Azure Subscriptions

```csharp
# Get Subscriptions
#=====================================================
az subscription list
# Get user associated with subscriptions
az account list --query "[].{Name:name, User:user.name}"
$sub = az account show --query id --output tsv

```

