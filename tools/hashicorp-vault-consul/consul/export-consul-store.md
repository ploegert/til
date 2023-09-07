---
description: The following will export the consul key/values to a file.
---

# Export Consul Store



```csharp
[CmdletBinding()] 
Param(
    [string] $outpath = "d:\data",
    [string] $outfile= "consul.json",
    [string] $path_target = "SOME_APP/SOME_ENV/"
  )

BEGIN {
  ## Vault CLI Basics

  # Setup for required stuffs: 
  #Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
  #choco install consul

  # Setup Env Variables
  $consul = @{url = $env:CONSUL_HTTP_ADDR; token=$env:CONSUL_HTTP_TOKEN }
  

  # $env:CONSUL_HTTP_ADDR = $consul.url
  # $env:CONSUL_HTTP_TOKEN = $consul.token

}
Process
{

  $v = consul kv export $path_target
  $v | convertfrom-json | convertto-json | out-file "$outpath\$outfile"
  $c = get-content "$outpath\$outfile" | convertfrom-json #-AsHashtable
  $list = New-Object System.Collections.ArrayList

  write-host "key is $($c.key)"
  write-host "val is $($c.value)"
  # break

  if ((get-host).version.major -eq "5")
  {
    # post 5.x
    $c.value | % {
      $te = split-path $_.key -parent
      $p1 = $te.Replace('\','/')
      $k1 = split-path $_.key -leaf
      $v1 =  [Text.Encoding]::Utf8.GetString([Convert]::FromBase64String($_.value)) 
      
      $object = @{ "path" = $p1; "key" = $k1; "val" =  $v1; }
      $list.add($object) | out-null

      #write-host "$($p1) $($k1) ==> $($v1)"
      write-host ("{0,-70} {1,-40} ==> {2,-20}" -f $p1,$k1,$v1) -ForegroundColor yellow
    }

  }
  else {
    # Posh 7.x
    $c | % {
      $key = $_.key
      $value = $_.value

      $property = ""
      $p1 = ""
      $v1 = ""

      $a = $key
      write-debug "key is: $a"
      $property = split-path $a -leaf
      $p1 =  split-path $a -parent
      $v1 =  [Text.Encoding]::Utf8.GetString([Convert]::FromBase64String($value)) 

      $object = @{ "path" = $p1; "key" = $property; "val" =  $v1; }
      $list.add($object) | out-null

      write-host ("{0,-70} {1,-40} ==> {2,-20}" -f $p1,$property,$v1) -ForegroundColor yellow

    }
  }

  $file = "$outpath\consul.json"
  $list | convertto-json | out-file $file
  
  write-host "Done." -foregroundcolor green


}

END {
    write-host "Done." -foregroundcolor green
}

```

