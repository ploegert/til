# Config ubuntu 20.04 vm for rdp

The following should get you up and running so you can rpd into your local hyperv vm

```bash
sudo apt update

# Useful Pre-Reqs
sudo apt-get install unzip

# Install virtual Drivers
sudo apt install -y linux-tools-virtual
sudo apt install -y linux-cloud-tools-virtual

# Run Config Script to download & configure RDP parts
cd Downloads/
wget https://raw.githubusercontent.com/Hinara/linux-vm-tools/ubuntu20-04/ubuntu/20.04/install.sh
sudo chmod +x install.sh
sudo ./install.sh
init 6
cd Downloads/
sudo ./install.sh 
sudo apt update

#Reboot again just to be sure
init 6
```

### Virtual Box Additions

If you're using Virtual Box, you may want to add the Guest Additions. Youn the following command first, before mounting.

Reference here: [https://itsfoss.com/virtualbox-guest-additions-ubuntu/](https://itsfoss.com/virtualbox-guest-additions-ubuntu/)

```
sudo apt install build-essential dkms linux-headers-generic 
```

### Additional Tools

Add some Additional Tools

```
#======================================
# Install Edge
cd Downloads/
sudo apt install ./microsoft-edge-beta_97.0.1072.21-1_amd64.deb 
sudo apt-get install libatomic1

#======================================
# Setup VSCode Repos
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/trusted.gpg.d/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
rm -f packages.microsoft.gpg

# Install VSCode
sudo apt install apt-transport-https
sudo apt update
sudo apt install code # or code-insiders
```

