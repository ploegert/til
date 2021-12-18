# Setup an Ubuntu 20.04 vm

Open up Hyper-V Manager, and create "**New" vm**

****![](<../../.gitbook/assets/image (32).png>)****

Select **Next:**

****![](<../../.gitbook/assets/image (23).png>)

**Type in a Name and then select Next**

****![](<../../.gitbook/assets/image (35).png>)****

Then Select **"Generation 2"**

****![](<../../.gitbook/assets/image (22).png>)****

Put in the amount of memory you'd like to use. I use **2048**

****![](<../../.gitbook/assets/image (39).png>)****

Configure your networking by selecting **"Default"**

****![](<../../.gitbook/assets/image (25).png>)****

Nothing to change here, select **"Next"**

****![](<../../.gitbook/assets/image (24).png>)****

Here you will want to select the iso image you'll be using. loaded my iso file from 20.04 LTS from this url: [https://ubuntu.com/download/desktop](https://ubuntu.com/download/desktop)

****![](<../../.gitbook/assets/image (36).png>)****

Select the image file

****![](<../../.gitbook/assets/image (31).png>)****

****

Then select **finish** from here

****![](<../../.gitbook/assets/image (34).png>)****

****

### Configure VM Settings

Now we have created the vm, we'll want to tweak some settings.

Right click on your vm and select "**settings**"

![](<../../.gitbook/assets/image (21).png>)

Then go and select "**Security**" tab

****![](<../../.gitbook/assets/image (38).png>)****

And then select **"Microsoft UEFI Certificate Authority**",&#x20;

****![](<../../.gitbook/assets/image (27).png>)****

Then select **"Integration Services"**, and check **"Guest Services"**

****![](<../../.gitbook/assets/image (33).png>)****

Then click "**Checkpoints"** section and disable **"Enable Checkpoints"**

****![](<../../.gitbook/assets/image (37).png>)****

Then select "**OK"** to close

****![](<../../.gitbook/assets/image (29).png>)****

You should now be able to run your image.

### Launching the VM

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

****
