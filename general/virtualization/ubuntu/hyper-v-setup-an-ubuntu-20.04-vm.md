# Hyper-V - Setup an Ubuntu 20.04 vm

Open up Hyper-V Manager, and create "**New" vm**

![](<../../../.gitbook/assets/image (39) (1).png>)

Select **Next:**

![](<../../../.gitbook/assets/image (26) (1).png>)

**Type in a Name and then select Next**

![](<../../../.gitbook/assets/image (46) (1).png>)

Then Select **"Generation 2"**

![](<../../../.gitbook/assets/image (25) (1).png>)

Put in the amount of memory you'd like to use. I use **2048**

![](<../../../.gitbook/assets/image (55) (1).png>)

Configure your networking by selecting **"Default"**

![](<../../../.gitbook/assets/image (28) (1) (1).png>)

Nothing to change here, select **"Next"**

![](<../../../.gitbook/assets/image (27) (1).png>)

Here you will want to select the iso image you'll be using. loaded my iso file from 20.04 LTS from this url: [https://ubuntu.com/download/desktop](https://ubuntu.com/download/desktop)

![](<../../../.gitbook/assets/image (47) (1) (1).png>)

Select the image file

![](<../../../.gitbook/assets/image (37) (1).png>)



Then select **finish** from here

![](<../../../.gitbook/assets/image (44) (1).png>)



### Configure VM Settings

Now we have created the vm, we'll want to tweak some settings.

Right click on your vm and select "**settings**"

![](<../../../.gitbook/assets/image (23) (1).png>)

Then go and select "**Security**" tab

![](<../../../.gitbook/assets/image (52) (1).png>)

And then select **"Microsoft UEFI Certificate Authority**",&#x20;

![](<../../../.gitbook/assets/image (31) (1).png>)

Then select **"Integration Services"**, and check **"Guest Services"**

![](<../../../.gitbook/assets/image (41) (1).png>)

Then click "**Checkpoints"** section and disable **"Enable Checkpoints"**

![](<../../../.gitbook/assets/image (50) (1).png>)

Then select "**OK"** to close

![](<../../../.gitbook/assets/image (34) (1).png>)

You should now be able to run your image.

### Launching the VM

Right click on your vm in Hyperv-Manager, and then select "Run"

![](<../../../.gitbook/assets/image (36) (1).png>)

Then select "**Install**" in the ubuntu dialog that pops up:

![](<../../../.gitbook/assets/image (24) (1).png>)

On Keyboard Layouts, just select the default and select "**continue"**:

![](<../../../.gitbook/assets/image (32) (1).png>)

Select Updates by selecting "**Install third-party software for graphics and wifi hardware and additional media formats**", then select "**Configure Secure Boot**" and enter your secure password.

![](<../../../.gitbook/assets/image (22) (1) (1).png>)

On Installation type, lets select "**Advanced Features"**

![](<../../../.gitbook/assets/image (48) (1).png>)

Check the **"Use LVM with the new Ubuntu installation",** and select **"Encrypt the new Ubuntu installation for security"** and then select **"OK"**

![](<../../../.gitbook/assets/image (40) (1).png>)

Then select **"Install Now"**

![](<../../../.gitbook/assets/image (19) (1).png>)

Now you'll enter a security key, and select recovery key as shown below

![](<../../../.gitbook/assets/image (51) (1).png>)

Now you'll elect to write the changes to disk by selecting continue:

![](<../../../.gitbook/assets/image (38) (1).png>)



Now, you'll select your time zone:

![](<../../../.gitbook/assets/image (30) (1).png>)

Now you'll enter the information about the desktop, such as your name, password, and machine name:



![](<../../../.gitbook/assets/image (45) (1).png>)

You should now see it installing & configuring...

![](<../../../.gitbook/assets/image (53) (1).png>)

YOu'll now be prompted to restart your machine:

![](<../../../.gitbook/assets/image (54) (1).png>)



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

