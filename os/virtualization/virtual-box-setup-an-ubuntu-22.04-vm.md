---
description: >-
  This setup calls out how to setup an Ubuntu 22.04 vm in virtual box (6.1.36)
  around the time frame of late 2022 running on a windows 11 host with Hyper-V
  configured.
---

# Virtual Box - Setup an Ubuntu 22.04 vm

## Create the initial VM

&#x20;Launch **Oracle VM Virtual Box Manager**, and create "**New" vm**

****<img src="../../.gitbook/assets/image (20).png" alt="" data-size="original">****

****<img src="../../.gitbook/assets/image (22).png" alt="" data-size="original">****

****![](<../../.gitbook/assets/image (52).png>)****

****

****![](<../../.gitbook/assets/image (23).png>)****

****![](<../../.gitbook/assets/image (24).png>)****

****

****

****

****

****

****

****![](<../../.gitbook/assets/image (21).png>)****

****

****

****

****

****

****![](<../../.gitbook/assets/image (43).png>)****

****

**Select Advanced**

****![](<../../.gitbook/assets/image (44).png>)****

****

### Launching the VM

Right click on your vm in Hyperv-Manager, and then select "Run"

Then select "**Install**" in the ubuntu dialog that pops up:

<figure><img src="../../.gitbook/assets/image (61) (1).png" alt=""><figcaption></figcaption></figure>

On Keyboard Layouts, just select the default and select "**continue"**:

<figure><img src="../../.gitbook/assets/image (56) (1).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>



On Installation type, lets select "**Advanced Features".**

<figure><img src="../../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

Check the **"Use LVM with the new Ubuntu installation",** and select **"Encrypt the new Ubuntu installation for security"** and then select **"OK"**

Then select **"Install Now"**

Now you'll enter a security key, and select recovery key as shown below

<figure><img src="../../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

Now, you'll select your time zone:

<figure><img src="../../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

Now you'll enter the information about the desktop, such as your name, password, and machine name:

<figure><img src="../../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

You should now see it installing & configuring...

<figure><img src="../../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

Y**o**u'll now be prompted to restart your machine:

<figure><img src="../../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>



###

### Virtual Box Additions

If you're using Virtual Box, you may want to add the Guest Additions. You will want to run the following command first, before mounting.

References:&#x20;

* [https://itsfoss.com/virtualbox-guest-additions-ubuntu/](https://itsfoss.com/virtualbox-guest-additions-ubuntu/)
* [https://linuxconfig.org/install-virtualbox-guest-additions-on-linux-guest](https://linuxconfig.org/install-virtualbox-guest-additions-on-linux-guest)

First, Goto **Devices->Insert Guest Additions CD image...**

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Then you'll want to run the following

```
# Install prereqs
sudo apt update
sudo apt install build-essential dkms linux-headers-generic 
sudo apt install build-essential dkms linux-headers-$(uname -r)

# after mounting the tools cd - use your username instead of "justin"
cd /media/justin
sudo ./VBoxLinuxAdditions.run

# then, reboot
reboot
shutdown -h now
```

<figure><img src="../../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

To verify Successful installation, run the following:

```
lsmod | grep vboxguest
```

<figure><img src="../../.gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>

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

****
