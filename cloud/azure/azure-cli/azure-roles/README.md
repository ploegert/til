# Azure Roles

## Setting up azure roles on subscriptions:

```csharp

# Log out and clear
az logout
az account clear

# Login as your global/high-privileged account to start creating service principals
az login

# Set base variables so we can create RBAC
export ROLE="owner" # this is overly privileged, so you should choose something more specific for your needs
export SP_NAME="your-service-principal-name"
export SUBSCRIPTION_ID="your-subscription-id"

az ad sp create-for-rbac \
  --name $SP_NAME \
  --role $ROLE \
  --scopes /subscriptions/$SUBSCRIPTION_ID

# Export variables to environment so the programmatic user can be used
export AZURE_SUBSCRIPTION_ID=$SUBSCRIPTION_ID
export AZURE_TENANT_ID='your-tenant-id'
export AZURE_CLIENT_ID='qwerty' # appId from create-for-rbac step
export AZURE_CLIENT_SECRET='qwerty' # password from create-for-rbac step


export SP_ROLE_ID="some-app-id" # appId from create-for-rbac step
export ROLE="owner" # this is overly privileged, so you should choose something more specific for your needs
export SUBSCRIPTION_ID="your-subscription-id"
```

## Create Role Assignment Manually

```csharp
$sub = ""
$role = "Owner"
az role assignment create --assignee-object-id $SP_ROLE_ID --role $role --scope /subscriptions/$sub

```

## Using script to Get user assignments:

```
$list = @("sub_name_1","sub_name_2","sub_name_3")
$user_id = "SOMEUSER"

$list | % {
    $sub_name = $_
    write-host "Setting Subscription for $sub_name..." -ForegroundColor yellow
    
    az account set --subscription $sub_name

    write-host "Setting Subscription..." -ForegroundColor yellow
    $sub = az account show --query id --output tsv
        
    $id = az ad user show --id $user_id" --query "objectId" --output tsv
    # $id = az ad user show --id "cploegj@jci.com" --query "objectId" --output tsv
    # $id = az ad user show --id "csheffmi@jci.com" --query "objectId" --output tsv

    write-host "sub is: $sub"
    write-host "id is $id"

    #az role assignment create --assignee-object-id $id --role $role --scope /subscriptions/$sub

}
```

## Get Role Assignemnts

```csharp
$user_id = "SOME_USER"
$sub     = "SOME_SUB"
$role    = "Owner"

# Get Role Id:
#==========================================================
az role definition list --name "Owner"
az role definition list --output tsv --query '[].{roleName:roleName}'


az role definition list --query "[].{name:owner, roleType:roleType, roleName:roleName}" --output tsv
az role assignment list --output json --query '[].{principalName:principalName, roleDefinitionName:owner, scope:scope}'


az account list --query '[?contains(name,"$sub")].{name:name, id:id}'
az account list --query "[?name=='$sub'].{Name:name, Id:id}"
az account list --query "[?name=='$sub']" -tsv


az role assignment create --assignee-object-id $SP_ROLE_ID --role $role --scope /subscriptions/$sub

az role definition list --output tsv --query '[].{roleName:roleName}'
```
