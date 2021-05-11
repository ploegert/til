# Backup Databse

### Powershell code:

Note: You'll need to make sure you have pg\_dump installed

```csharp
#Backup
[Cmdletbinding()]
Param(
    # Environment name
    [Parameter()]
    [ValidateNotNullOrEmpty()]
    [string]$DeploymentStage,
    [String]$PGPWD
)

$EnvironmentSuffix = $DeploymentStage.Substring(0,1)

$env:PGPASSWORD = "$PGPWD"
$env:PGSSLMODE="allow"
$user = "citus"
$svr = "SOMESERVER.postgres.database.azure.com"
$db = "citus"

$schemas = @("cron","schema1","schema2","schema3","schema4")

foreach ($schema in $schemas)
{
    write-host "Processing $schema"
    pg_dump --host $svr --file ./$schema.sql --username $user --verbose --format=c --blobs --schema $schema $db
}
```

