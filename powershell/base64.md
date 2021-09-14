# Base64



```bash
$path = "./certs/obtdz1-root-public.cer"
$file = [System.Text.Encoding]::UTF8.GetBytes((get-content ($path)))
$certText = [Convert]::ToBase64String($file)
```



