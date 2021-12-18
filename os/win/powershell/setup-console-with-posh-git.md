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

If you have issues and see blocks in the terminal, you may need to make sure what fonts are loaded and possibly edit your vs code settings:

![](<../../../.gitbook/assets/image (20) (1).png>)

![](<../../../.gitbook/assets/image (19) (1).png>)

References:

* [https://ohmyposh.dev/docs/fonts](https://ohmyposh.dev/docs/fonts)
* [https://docs.microsoft.com/en-us/windows/terminal/tutorials/powerline-setup](https://docs.microsoft.com/en-us/windows/terminal/tutorials/powerline-setup)

