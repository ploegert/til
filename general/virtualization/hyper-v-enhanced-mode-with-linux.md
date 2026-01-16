---
icon: linux
---

# Hyper-V Enhanced mode with Linux

### Inspirational links that helped me along the path: <a href="#anc_3" id="anc_3"></a>

* [https://www.nakivo.com/blog/install-ubuntu-20-04-on-hyper-v-with-enhanced-session/](https://www.nakivo.com/blog/install-ubuntu-20-04-on-hyper-v-with-enhanced-session/)
  * _Credit where credit due - this TIL borrows heavily from this post_
* [https://learn.microsoft.com/en-us/troubleshoot/windows-server/virtualization/usb-device-hyper-v-virtual-machine](https://learn.microsoft.com/en-us/troubleshoot/windows-server/virtualization/usb-device-hyper-v-virtual-machine)
* **SmartCard related chaos**
  * [https://documentation.ubuntu.com/server/how-to/security/smart-card-authentication/](https://documentation.ubuntu.com/server/how-to/security/smart-card-authentication/)
  * [https://linuxvox.com/blog/install-vm-tools-ubuntu/](https://linuxvox.com/blog/install-vm-tools-ubuntu/)

#### Running Commands to get the basics setup <a href="#running-commands-to-get-the-basics-setup" id="running-commands-to-get-the-basics-setup"></a>

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

## Ubunut 20.04
wget https://raw.githubusercontent.com/ploegert/linux-vm-tools/refs/heads/master/ubuntu/20.04/install.sh

# Ubuntu 22.04
https://raw.githubusercontent.com/ploegert/linux-vm-tools/refs/heads/master/ubuntu/22.04/install.sh

# Ubuntu 24.04
https://raw.githubusercontent.com/ploegert/linux-vm-tools/refs/heads/master/ubuntu/24.04/install.sh

sudo chmod +x install.sh
sudo ./install.sh
init 6
cd Downloads/
sudo ./install.sh 
sudo apt update

#Reboot again just to be sure
init 6
```

#### Virtual Box Additions <a href="#virtual-box-additions" id="virtual-box-additions"></a>

If you're using Virtual Box, you may want to add the Guest Additions. Youn the following command first, before mounting.

Reference here: [https://itsfoss.com/virtualbox-guest-additions-ubuntu/](https://itsfoss.com/virtualbox-guest-additions-ubuntu/)

```
sudo apt install build-essential dkms linux-headers-generic 
```

### Enhanced Session Configuration on the Host Windows Machine Running Hyper-V <a href="#anc_3" id="anc_3"></a>

You have to allow the enhanced session mode in general Hyper-V settings. Otherwise, the enhanced session mode icon will be inactive in the VM window. Open Hyper-V Manager, right-click the name of your host Windows machine on which Hyper-V is installed, and, in the context menu, click **Hyper-V Settings**.

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

There are two different settings that have "_**Enhanced Session Mode Policy."** &#x53;_&#x65;lect the **Allow enhanced session mode** checkbox on both. Hit **OK** to save Hyper-V settings and close the window.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>



{% hint style="info" %}
<mark style="color:$danger;">Before you go any further -</mark> <mark style="color:$danger;"></mark><mark style="color:$danger;">**MAKE SURE YOUR VM IS SHUT DOWN!**</mark>
{% endhint %}

Now before you start up the VMs, you should set Hyper-V to enable the enhanced session mode using the HvSocket for the “Ubuntu Hyper-V” VM on which Ubuntu 20.04 is installed.

Run this command in Terminal/posh (as administrator) on the host Windows machine running Hyper-V:

&#x20;    `Set-VM -VMName <your_vm_name>  -EnhancedSessionTransportType HvSocket`

Use double quotas if the VM name contains spaces. In my case the command is:

&#x20;    `Set-VM -VMName "Ubuntu Hyper-V" -EnhancedSessionTransportType HvSocket`

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

### Connecting to Enhanced Session

You'll know that you made progress if when you launch your vm, you get a dialog that asks you to set the terminal size:

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

If your goal is to connect USB accessories (perhaps a Yubikey?), then you'll want to make sure you select --> Show Options, and select "**Local Resources**"

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Then select "**More**", because oh boy do you want more!

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>



Then select the checkbox for "**Other supported Plug and Play (PnP) devices**" - if you want, that is.&#x20;

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

Now when you select "Connect," you should get a dialog that says "Login to ..."

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Simply type in your username and password, and you should feel joy flowing through your human veins (you are human, right?).

When you look at your view, you should see "Enhanced Session" checked:

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

#### Smart Card Authentication <a href="#smart-card-authentication" id="smart-card-authentication"></a>

Smart card authentication extends certificate-based methods by introducing a physical token that stores user certificates. When the card is inserted into a reader, the system retrieves the certificates and performs validation.

Configuring SmartCard support involves setting up the necessary libraries and modules to enable certificate-based authentication using physical tokens. There are various SmartCard solutions available, such as YubiKey, which can be integrated with various Linux distributions. For instructions on the two supported platforms, refer to the distribution documentation:

* [Ubuntu SmartCard configuration](https://documentation.ubuntu.com/server/how-to/security/smart-card-authentication/)
* [Red Hat Enterprise Linux SmartCard configuration](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_smart_card_authentication/index)
* [YubiKey SmartCard configuration](https://developers.yubico.com/pam-u2f/)
* [OpenSC SmartCard configuration](https://github.com/OpenSC/OpenSC/wiki)
* [PKCS#11 configuration reference](https://p11-glue.github.io/p11-glue/p11-kit/manual/pkcs11-conf.html)

#### Example Smart Card configuration <a href="#example-smart-card-configuration" id="example-smart-card-configuration"></a>

The following steps configure a reference example of using the YubiKey/Edge bridge integration, but other smart card providers can be configured similarly.

1.  Install Smart Card drivers and YubiKey support:

    ```bash
    sudo apt install pcscd yubikey-manager
    ```
2.  Install YubiKey/Edge Bridge components:

    ```bash
    sudo apt install opensc libnss3-tools openssl
    ```
3.  Configure Network Security Service (NSS) database for the current user:

    ```bash
    mkdir -p $HOME/.pki/nssdb
    chmod 700 $HOME/.pki
    chmod 700 $HOME/.pki/nssdb
    modutil -force -create -dbdir sql:$HOME/.pki/nssdb
    modutil -force -dbdir sql:$HOME/.pki/nssdb -add 'SC Module' -libfile /usr/lib/x86_64-linux-gnu/pkcs11/opensc-pkcs11.so
    ```

#### Certificate-Based Authentication <a href="#certificate-based-authentication" id="certificate-based-authentication"></a>

Certificate-based client authentication is implemented through the Secure Sockets Layer (TLS/SSL) protocol. In this process, the client signs a randomly generated data block with its private key, then transmits both the certificate and the signed data to the server. The server checks the signature and validates the certificate before granting access.

The easiest way to configure Certificate-Based Authentication (CBA) is to use a Private Key Infrastructure (PKI) solution that issues user certificates to Linux devices. These certificates can then be used for authentication against Microsoft Entra ID. To configure Linux to accept these certificates for authentication, you typically need to set up the appropriate certificate stores and ensure that the system's authentication mechanisms are configured to use these certificates.

