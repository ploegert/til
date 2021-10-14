# Setup console with posh-git



```
winget install JanDeDobbeleer.OhMyPosh

Install-Module posh-git -Scope CurrentUser
Install-Module oh-my-posh -Scope CurrentUser -RequiredVersion 2.0.412

# If you are using PowerShell Core, install PSReadline:
Install-Module -Name PSReadLine -Scope CurrentUser -Force -SkipPublisherCheck

Import-Module posh-git
Import-Module oh-my-posh
Set-PoshPrompt -Theme paradox
```



References:

* [https://ohmyposh.dev/docs/fonts](https://ohmyposh.dev/docs/fonts)
* [https://docs.microsoft.com/en-us/windows/terminal/tutorials/powerline-setup](https://docs.microsoft.com/en-us/windows/terminal/tutorials/powerline-setup)

