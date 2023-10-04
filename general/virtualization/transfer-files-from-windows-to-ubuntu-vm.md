---
description: Need to copy a file from a windows hope to a linux vm? We got you!
---

# Transfer files from Windows to Linux VM

## Setup Linux

### Ubuntu

First, install Openssh on the vm:

```bash
sudo apt-get install openssh-server openssh-client
```

### RHEL

Install the SSH server package `openssh` by using the `dnf` commands

```bash
# Install Package
sudo dnf install openssh-server

# Start the sshd daemon and set to start after reboot
sudo systemctl start sshd
sudo systemctl enable sshd

# Confirm that the sshd daemon is up and running:
sudo systemctl status sshd

# Open the SSH port 22 to allow incoming traffic:
sudo firewall-cmd --zone=public --permanent --add-service=ssh
sudo firewall-cmd --reload

# Optionally, locate the SSH server man config file /etc/ssh/sshd_config and perform custom configuration.
# Every time you make any change to the /etc/ssh/sshd_config configuration file reload the sshd service to apply changes:
sudo systemctl reload sshd
```



***

### &#x20;Setup Windows

### Install WinSCP from [https://winscp.net/downloads.php](https://winscp.net/downloads.php)

Get the IP of Linux VM:

```bash
ifconfig
```

<figure><img src="../../.gitbook/assets/image (69) (1).png" alt=""><figcaption></figcaption></figure>

Launch WinSCP, and setup the necessary details of the HostName = IP, username, pass

<figure><img src="../../.gitbook/assets/image (72) (1).png" alt=""><figcaption></figcaption></figure>

Accept the ssh key

<figure><img src="../../.gitbook/assets/image (70) (1).png" alt=""><figcaption></figcaption></figure>

Navigate, Drag & Drop

<figure><img src="../../.gitbook/assets/image (71) (1).png" alt=""><figcaption></figcaption></figure>
