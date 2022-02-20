# AzureAD.Get-Devices

In the following we use powershell AzureAD module to get a list of devices & details similar to what you would see in the Azure AD Portal:

```powershell
$PathCsv = "~\Desktop\deviceList.csv"

Install-Module AzureAD
Import-Module AzureAD

Connect-AzureAD

$deviceList = Get-AzureADDevice -All 1 -Filter "startswith(DeviceOSType,'Linux')" | select DisplayName, accountEnabled, deviceOSType, deviceOSVersion, registeredOwners, isCompliant, IsManaged, approximateLastLogonTimeStamp	, deviceId, deviceObjectVersion, devicePhysicalIds, dirSyncEnabled, lastDirSyncTime, objectId, objectType, DeviceTrustType,  ProfileType, extensionAttributes, deviceMetadata

$devices = @()
$count = 0 

$export_time = Get-Date -Format "MM/dd/yyyy HH:mm K"
   
foreach($device in $deviceList){
    $deviceOwner = $device | Get-AzureADDeviceRegisteredOwner
    #$deviceUser = $device | Get-AzureADDeviceRegisteredUser
    #$device | (Get-AzureADUserExtension).Get_Item("createdDateTime") 

    $deviceProps = [ordered] @{
        # Sampled = "02/18/2022 12:00 -06:00"
        Sampled = $export_time

        # Name  = "Blahblahblah"
        Name = $device.DisplayName.replace("`n",", ").replace("`r",", ")
        
        # Enabled = True | False
        Enabled = if ([string]::IsNullOrEmpty($device.AccountEnabled))
                    { "No" }
                  else {
                    if (($device.AccountEnabled -eq $true) -or ($device.AccountEnabled -eq "True"))
                        { "Yes" }
                    else
                        { "No" }
                  }

        # OS = Linux
        OS = $device.DeviceOSType

        # Version = "20.4.0"
        Version = $device.DeviceOSVersion
        
        # JoinType = "Azure AD registered"
        JoinType = if ([string]::IsNullOrEmpty($device.DeviceTrustType))
                        { "Azure AD registered" }
                   else 
                   {
                       if ($device.DeviceTrustType -eq "Workplace")
                            { "Azure AD registered" }
                        else
                            { $device.DeviceTrustType}
                   }

        # Owner = "FNAME LNAME"
        Owner = $deviceOwner.DisplayName.replace("`n",", ").replace("`r",", ")

        # MDM = None | Microsoft Intune
        MDM = if ([string]::IsNullOrEmpty($device.IsManaged))
                { "None" }
            else
                { 
                    if ($device.IsManaged)
                        { "Microsoft Intune" }
                    else
                        { "None" }
                 }        
        
        # Compliant = No | Yes
        Compliant = if (($device.IsManaged) -and $device.isCompliant)
                { "Yes" }
            elseif (($device.IsManaged) -and (-not $device.isCompliant))
                { "No" }
            else
                { "N/A" }

        LastLogonTimestamp = $device.ApproximateLastLogonTimeStamp
    }

    $deviceObj = New-Object -Type PSObject -Property $deviceProps
    $devices += $deviceObj

    write-host "[$count] Processed $($deviceObj.Owner) for device $($deviceObj.Name)"
    $device
    #$devices | ft
    #$deviceOjb | ft
    $count = $count + 1
    
    #if ($count -gt 82)  {break}
    
}
$devices | ft
$deviceOjb | ft
   
$devices | Export-Csv -Path $PathCsv -NoTypeInformation -Append
```

