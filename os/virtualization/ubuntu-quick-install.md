# Ubuntu Quick Install

Install VSCode

```bash
sudo apt install software-properties-common apt-transport-https wget
wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
sudo apt install code

sudo apt update
sudo apt upgrade
```

### Install Network Tools (ifconfig)

```bash
sudo apt install net-tools
```

### Install SSH

```
sudo apt-get install openssh-server openssh-client
```
