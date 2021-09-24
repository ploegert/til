# Setup VPN Certificates

### Setup Variables

```text
$app = "obt"
$env = "c"
$zone = "z1"
$name = "$($app)-vpn-root"
$sub = "CN=$name"
$pass = "SOMEPASS"
$path = "C:\vso\Deploy\certs"
$file_root_pfx = (join-path $path "$name.pfx")
$file_root_cer = (join-path $path "$name.cer")
$pwd = ConvertTo-SecureString -String $pass -Force -AsPlainText

$ag = @{
    Type                = "Custom";
    KeySpec             = "Signature";
    Subject             = $sub
    KeyExportPolicy     = "Exportable";
    HashAlgorithm       = "sha256";
    KeyLength           = 2048;
    CertStoreLocation   = "Cert:\CurrentUser\My";
    KeyUsageProperty    = "Sign";
    KeyUsage            = "CertSign";
}

```

## Create Cert

```text
$cert = New-SelfSignedCertificate @ag
```

## Verify Cert

```text
(Get-ChildItem -Path Cert:\CurrentUser\My | Where-Object {$_.Subject -match $sub})
```

## Export  Private Key

```text
$cert | Export-PfxCertificate -FilePath $file_root_pfx -Password $pwd
```

## export Public Key

```text
$cert | Export-Certificate -FilePath $file_root_cer $b64_root_cer = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes((get-content $file_root_cer)))
write-host "Cert output is: $b64_root_cer" -foregroundcolor magenta
```



### Export User Certs

```text
$users = @(
   @{"user"="user1";  "pass"="SOMEPASSWORD";},
   @{"user"="user2";    "pass"="SOMEPASSWORD";}
)
$users_cert= @{
    Type="Custom";
    DnsName="P2SChildCert";
    KeySpec="Signature";
    Subject="CN=$($app)-vpn-$usr";
    KeyExportPolicy="Exportable";
    HashAlgorithm="sha256";
    KeyLength=2048; 
    CertStoreLocation="Cert:\CurrentUser\My";
    Signer=$cert;
    TextExtension=@("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
}

$gen_cert = New-SelfSignedCertificate @users_cert
#$cert | Export-Certificate -FilePath (join-path $path "$usr.cer")
$pwrd_secure = ConvertTo-SecureString -String $pwrd -Force -AsPlainText

$out_path = (join-path $path "$($app)-vpn-$usr.pfx")
$gen_cert | Export-PfxCertificate -FilePath $out_path -Password $pwrd_secure
```

