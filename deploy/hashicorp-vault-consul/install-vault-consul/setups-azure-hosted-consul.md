# Setups Azure Hosted Consul



### Activating consul in Azure

```text
# ensure you have the latest az cli: https://aka.ms/installazurecliwindows


# add the hashicorp extension:
az extension add --source https://releases.hashicorp.com/hcs/0.2.0/hcs-0.2.0-py2.py3-none-any.whl


# Get config    
$rg = "core-rg" 
$name = "appdcorehashi"

az hcs get-config --resource-group $rg --name $name

# check config:
cat consul.json

# bootstrap acl system
az hcs create-token --resource-group $rg --name $name
        Command group 'hcs' is in preview. It may be changed/removed in a future release.
        {
        "masterToken": {
            "accessorId": "95940b0b-afb4-8d0b-1f66-5a4463f72954",
            "secretId": "SOMEPASS"
        }
        }
```

