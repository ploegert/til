---
description: A module that allows you to log out to standard output with colors
---

# Logging.psm1

```text
#==============================================================================================
#region Logging Functions        
function Write-Log
{
    [cmdletbinding()]
    param([Parameter(Mandatory=$true, Position=0, ValueFromPipeline=$true)][AllowEmptyString()][string]$Message)
    Write-Verbose  ("[{0:s}] [$(get-caller)] {1}`r`n" -f (get-date), $Message)
}

function write-ok($text="")
    { write-emoji -code "u+2705" -text $text }
function write-done($text="")
    { write-emoji -code "u+2705" -text "done. $text" }
function write-angry
    { write-emoji "u+1F975" }
function write-emoji($code='u+2705', $text="")
{
    $StrippedUnicode = $code -replace 'U\+',''
    $UnicodeInt = [System.Convert]::toInt32($StrippedUnicode,16)
    [System.Char]::ConvertFromUtf32($UnicodeInt) + " $text" 
}

function Write-Success($Message)
    { Write-Host (New-FormattedLog -Type "WIN" -Time (get-date) -Message $Message) -ForegroundColor Green }    
function Write-Fail($Message)
    { Write-Host (New-FormattedLog -Type "ERROR" -Time (get-date) -Message $Message) -ForegroundColor Red }    
function Write-Debug($Message)
    { Write-Host (New-FormattedLog -Type "DEBUG" -Time (get-date) -Message $Message) -ForegroundColor Cyan }
function Write-Normal($Message)
    { Write-Host (New-FormattedLog -Type "INFO" -Time (get-date) -Message $Message)  }
function Write-Yellow($Message)
    { Write-Host (New-FormattedLog -Type "INFO" -Time (get-date) -Message $Message) -ForegroundColor Yellow }    
function Write-Color($Message,$color)
    { Write-Host (New-FormattedLog -Type "INFO" -Time (get-date) -Message $Message) -ForegroundColor $color }
function New-FormattedLog ($Type, $Time, $Message)
    { ("[{0,-5}] [{1:s}] [{2}] {3}`r" -f $type, $Time, (get-caller 3), $Message) }
function get-caller($stackNum = 2)
    { process{ return ((Get-PSCallStack)[$stackNum].Command) } }

function write-compare
{
    [cmdletbinding()]
    Param($label,$expected,$actual)
    return ("{0,-15} Expected: {1,-45} ==> Actual: {2,-25}" -f $label, "[$expected]", "[$actual]" )
}
function write-header 
{
    [cmdletbinding()]
    Param($label,$color= "magenta", [switch]$min, $max = 100)
    $i = $label | measure-object -character | select -expandproperty characters
    $split = (($max-$i)/2)
    
    if (-not $min) { write-host "$('='*$max)" -foregroundcolor $color }
    write-host ("{0}  {1}  {2}" -f ('='*($split-4)),$label, ('='*$split)) -foregroundcolor $color
    if (-not $min) { write-host "$('='*$max)" -foregroundcolor $color }
}
#endregion

#==============================================================================================

Export-ModuleMember -Function Write-Log
Export-ModuleMember -Function write-ok
Export-ModuleMember -Function write-done
Export-ModuleMember -Function write-angry
Export-ModuleMember -Function write-emoji
Export-ModuleMember -Function Write-Success
Export-ModuleMember -Function Write-Fail
Export-ModuleMember -Function Write-Debug
Export-ModuleMember -Function Write-Normal
Export-ModuleMember -Function Write-Yellow
Export-ModuleMember -Function New-FormattedLog
Export-ModuleMember -Function get-caller
Export-ModuleMember -Function write-compar
Export-ModuleMember -Function write-header
#Export-ModuleMember -Function *
```

