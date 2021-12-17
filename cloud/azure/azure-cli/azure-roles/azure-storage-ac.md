# Azure Storage Ac

## Blob Backup

```csharp

#Backup
[Cmdletbinding()]
Param(
    # Environment name
    [Parameter()]
    [ValidateNotNullOrEmpty()]
    [String]$ConnectionString,
    [String]$Workspace
)



$AccountName = $ConnectionString.split(";")[1].Split("=")[1]
New-Item $Workspace\$AccountName -ItemType Directory
cd $Workspace\$AccountName
$SrcContext = New-AzureStorageContext -ConnectionString "$ConnectionString"
$containers = Get-AzureStorageContainer -Context $SrcContext

$containers
foreach ($Container in $containers.Name)
{
    New-Item $Workspace\$AccountName\$Container -ItemType Directory
    cd $Workspace\$AccountName\$Container
    write-host "Container name: $Container"
    Get-AzureStoracogeBlob -Container $Container -Context $SrcContext | Get-AzureStorageBlobContent
}
```

## Table Backup

```csharp
#Backup
[Cmdletbinding()]
Param(
    # Environment name
    [Parameter()]
    [ValidateNotNullOrEmpty()]
    [String]$ConnectionString
)

$AccountName = $ConnectionString.split(";")[1].Split("=")[1]
$SrcContext = New-AzureStorageContext -ConnectionString "$ConnectionString"

$SrcTables =  Get-AzureStorageTable -Context $SrcContext

$SrcTables | Sort-Object -Property "CloudTable" | Select-Object CloudTable,Uri | Export-CSV .\${AccountName}.csv -Encoding UTF8
```



## Backup using azcopy

```csharp
$backupDir = "D:\data\"
$AzCopyFolder = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\"
 
$storageAccountName = "SOME_STORAGE_ACCT"
$storageAccountKey = "SOME_KEY"
 
### Functions ###
 
function Init-StorageAccountTablesBackup {
    if(!(Test-Path $backupDir)) {
        Write-Host ("Creating folder $backupDir") -ForegroundColor Yellow
        New-Item -ItemType Directory -Path $backupDir
    }
    $ctx = New-AzureStorageContext $storageAccountName -StorageAccountKey $storageAccountKey
    $tables = Get-AzureStorageTable -Context $ctx
    $tables | Sort-Object -Property "Name" | Select-Object Name,Uri | Export-Csv "$backupDir\tables.csv" -Encoding UTF8
}
 
function Remove-StorageAccountTables {
    $tables = import-csv "$backupDir\tables.csv" -Encoding UTF8
    $ctx = New-AzureStorageContext $storageAccountName -StorageAccountKey $storageAccountKey
    foreach($t in $tables) {
        $tableName = $t.CloudTable
        Write-Host ("Removing table $tableName") -ForegroundColor Yellow
        Remove-AzureStorageTable –Name $tableName –Context $ctx
    }
}
 
function Backup-StorageAccountTables {
    $tables = import-csv "$backupDir\tables.csv" -Encoding UTF8
    cd $AzCopyFolder
    foreach($t in $tables) {
        $tableUrl = $t.Uri
        $tableName = $t.CloudTable
        Write-Host ("`nBacking up table $tableName") -ForegroundColor Yellow
        .\AzCopy.exe /Source:"$tableUrl" /Dest:"$backupDir" /SourceKey:"$storageAccountKey" /Manifest:"$tableName.manifest"
    }
}
 
function Restore-StorageAccountTables {
    $tables = import-csv "$backupDir\tables.csv" -Encoding UTF8
    cd $AzCopyFolder
    foreach($t in $tables) {
        $tableUrl = $t.Uri
        $tableName = $t.CloudTable
        Write-Host ("`nRestoring table $tableName") -ForegroundColor Yellow
        .\AzCopy.exe /Source:"$backupDir" /Dest:"$tableUrl" /DestKey:"$storageAccountKey" /Manifest:"$tableName.manifest" /EntityOperation:InsertOrReplace
    }
}


choco install azcopy -y


if(!(Test-Path $backupDir)) {
    Write-Host ("Creating folder $backupDir") -ForegroundColor Yellow
    New-Item -ItemType Directory -Path $backupDir
}
$ctx = New-AzureStorageContext $storageAccountName -StorageAccountKey $storageAccountKey
$tables = Get-AzureStorageTable -Context $ctx

cd $AzCopyFolder
foreach($t in $tables) {
    $tableUrl = $t.Uri
    $tableName = $t.Name
    Write-Host ("`nBacking up table $tableName") -ForegroundColor Yellow
    .\AzCopy.exe /Source:"$tableUrl" /Dest:"$backupDir" /SourceKey:"$storageAccountKey" /Manifest:"$tableName.manifest"
}
```
