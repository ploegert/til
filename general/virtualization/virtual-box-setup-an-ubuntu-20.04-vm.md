---
description: >-
  This setup calls out how to setup an Ubuntu 20.04 vm in virtual box (6.1.30)
  around the time frame of early 2022 running on a windows 10 host with Hyper-V
  configured.
---

# Virtual Box - Setup an Ubuntu 20.04 vm

## Create the initial VM

&#x20;Launch **Oracle VM Virtual Box Manager**, and create "**New" vm**

<img src="../../.gitbook/assets/image (20).png" alt="" data-size="original">

<img src="../../.gitbook/assets/image (22).png" alt="" data-size="original">

![](<../../.gitbook/assets/image (52).png>)



![](<../../.gitbook/assets/image (23).png>)

![](<../../.gitbook/assets/image (24).png>)













![](<../../.gitbook/assets/image (21).png>)











![](<../../.gitbook/assets/image (43).png>)



**Select Advanced**

![](<../../.gitbook/assets/image (44).png>)



### Launching the VM

Right click on your vm in Hyperv-Manager, and then select "Run"

Then select "**Install**" in the ubuntu dialog that pops up:

![](<../../.gitbook/assets/image (24) (1).png>)

On Keyboard Layouts, just select the default and select "**continue"**:

![](<../../.gitbook/assets/image (32).png>)

Select Updates by selecting "**Install third-party software for graphics and wifi hardware and additional media formats**", then select "**Configure Secure Boot**" and enter your secure password.

![](<../../.gitbook/assets/image (22) (1).png>)

On Installation type, lets select "**Advanced Features"**

![](<../../.gitbook/assets/image (48).png>)

Check the **"Use LVM with the new Ubuntu installation",** and select **"Encrypt the new Ubuntu installation for security"** and then select **"OK"**

![](<../../.gitbook/assets/image (40).png>)

Then select **"Install Now"**

![](<../../.gitbook/assets/image (19).png>)

Now you'll enter a security key, and select recovery key as shown below

![](<../../.gitbook/assets/image (51).png>)

Now you'll elect to write the changes to disk by selecting continue:

![](<../../.gitbook/assets/image (38).png>)



Now, you'll select your time zone:

![](<../../.gitbook/assets/image (30).png>)

Now you'll enter the information about the desktop, such as your name, password, and machine name:



![](<../../.gitbook/assets/image (45).png>)

You should now see it installing & configuring...

![](<../../.gitbook/assets/image (53).png>)

YOu'll now be prompted to restart your machine:

![](<../../.gitbook/assets/image (54).png>)



### Running Commands to get the basics setup

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

