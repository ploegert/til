# 3: Generate SSH Key

## 3: Generate a ssh key to be used for securing the deployment:

```text
# If windows:
Set-location ~ 
mkdir ~/.ssh

Ssh-keygen.exe


#      Generating public/private rsa key pair.
#      Enter file in which to save the key (/c/Users/joetest/.
#       ssh/id_rsa): /c/Users/joetest/.ssh/
#      Enter passphrase (empty for no passphrase):
#      Enter same passphrase again:
#      Your identification has been saved in /c/Users/joetest/
#      .ssh/
#      Your public key has been saved in /c/Users/joetest/.ssh/
#      The key fingerprint is:
#      #SHA256:jieniOIn20935n0awtn04n002HqEIOnTIOnevHzaI5nak
#      joetest@periwinkle
#      The key's randomart image is:

#      +---[RSA 2048]----+
#      |*o =.            |
#      |O*=*=.B       +-3|
#      |+* +             |
#      |    +   #$^  4   |
#      |             +.  |
#      | .o.ooo* o       |
#      |  .+o .          |
#      |   .=.           |
#      |   Eo      +*oo  |
#      +----[SHA256]-----+

#      $ dir .ssh
#      id_rsa  id_rsa.pub

$pub_key = get-content ~/.ssh/id_rsa.pub
```

Once this is done, then add the public key

### Convert root cert token for API gateway to base64

> Note: This only needs to be done once, adn then update vnet\_gw\_vpn\_cert\_name & vnet\_gw\_vpn\_cert\_data in the /terraform/base-env-setup/main.tf

```text
$path = "./certs/obtdz1-root-public.cer"
$file = [System.Text.Encoding]::UTF8.GetBytes((get-content ($path)))
$certText = [Convert]::ToBase64String($file)
```

