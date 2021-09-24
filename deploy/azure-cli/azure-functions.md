# Azure Functions

### Retrieve Function App Settings:

```csharp

$rg = "SOME_RG"
$svc = "SOME_SERVICE_NAME"


az functionapp config appsettings list  -g $rg -n $svc --setting-names


az functionapp config appsettings delete  -g $rg -n $svc --setting-names AzureWebJobsDashboard


```

## Get a list of Function Keys:

```csharp
$rg = "SOME_RG"
$func_name = "SOME_FUNCTION"

$a = az functionapp  list -o json
$Func_list= $a | convertfrom-json |  % { $_.name }
az functionapp keys list -g $rg -n $func_name -o json | convertfrom-json


```

