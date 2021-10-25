
The following describes how to use an Azure VM to install Ubuntu Desktop

```bash
sudo apt-get update
sudo apt-get install ubuntu-desktop

sudo apt install xrdp 
sudo adduser xrdp ssl-cert  

#sudo ufw allow from 192.168.33.0/24 to any port 3389
sudo ufw allow 3389

```

## Restart the VM
Restart the vm using the azure portal

## The next task that we need to do is to install and configure xRDP

Create a Downloads folder under your home directory, and go to there:

```bash
mkdir Downloads
cd Downloads
```

## Download
Download the next zip file (that contains the script to install and configure xRDP for Unity Interface).. I’ll give credits to Griffon who’s wrote the original one install-xrdp-1.9.1.zip
 
```bash
wget -O ~ https://tym1zq.bn.files.1drv.com/y4mWThEWEC0iKojSq34rQCdsw4GdG0dkbpLTLPMHJRT6AXndOq1f8rZ2VvTAi2g9lpg3CFrHKnG9XcF5FXPeR9FdOjdg9Oyvu8BIlxY4xXLR1HsI-1vRM4SIGr5Ik39gzKceZL3fqgROkVn3aMmYZEQI6xrFfVLYyMfC-RvBFjYGxLCNp8BpuqRu_LFVsVfAdi-/install-xrdp-1.9.1.zip

# Because I need it in the next step, install unzip
sudo apt-get install unzip

# Ok, now unzip the file to get the script
unzip install-xrdp-1.9.1.zip

# Change the file permissions by:
chmod +x ~/Downloads/install-xrdp-1.9.1.sh



sudo apt-get update
sudo apt-get -y install xfce4
sudo apt install xfce4-session

```
 




References:
- https://build5nines.com/how-to-setup-an-ubuntu-linux-vm-in-azure-with-remote-desktop-rdp-access/#:~:text=How%20to%20Setup%20an%20Ubuntu%20Linux%20VM%20in,Installing%20RDP%20Support%20on%20the%20Linux%20VM.%20
- https://sorcia25.wordpress.com/2018/03/02/how-to-setup-an-ubuntu-linux-vm-in-azure-with-remote-desktop-rdp-access-ubuntu-desktop-gui/
- https://linuxize.com/post/how-to-install-xrdp-on-ubuntu-20-04/
- https://docs.microsoft.com/en-us/azure/virtual-machines/linux/use-remote-desktop