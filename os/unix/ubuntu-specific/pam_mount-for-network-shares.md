# pam\_mount for network shares

If you have network shares in your environment that you want to automount on login without reconfiguring fstab entries then pam\_mount is a good candidate. This is especially useful if you have a centralised LDAP server which you use for authentication.

This tutorial assumes that you want to mount CIFS shares and are using a Debian based distribution

### Backup existing pam configuration <a href="#backup-existing-pam-configuration" id="backup-existing-pam-configuration"></a>

```
sudo cp /etc/pam.d/common-auth /etc/pam.d/common-auth.baksudo cp /etc/pam.d/common-session /etc/pam.d/common-session.bak
```

### Install required libraries <a href="#install-required-libraries" id="install-required-libraries"></a>

```
sudo apt install -y libpam-mount cifs-utils
```

### Enable user pam\_mount configuration <a href="#enable-user-pam_mount-configuration" id="enable-user-pam_mount-configuration"></a>

Now we will want to configure a user specific pam\_mount config. In order to do this we need to make some changes to the global `pam_mount.conf.xml` file located in `/etc/security`

Open the `pam_mount.conf.xml` in your chosen text editor and locate the following lines of text

```
<!--<luserconf name=".pam_mount.conf.xml" />-->
```

We will want to uncomment this by removing the leading and trailing comment lines and save the changes

```
<luserconf name=".pam_mount.conf.xml" />
```

### Create user pam\_mount configuration file <a href="#create-user-pam_mount-configuration-file" id="create-user-pam_mount-configuration-file"></a>

You can now make a user pam mount configuration file within your home directory. The file name has to be `.pam_mount.conf.xml`

Create the new configuration file with the following parameters (replacing the User, Mount Point, Path and Server configuration as required) and save the changes

```
<?xml version="1.0" encoding="utf-8" ?><pam_mount><volume options="nodev,nosuid" user="example-user" mountpoint="~/shares/share1" path="share1" server="dc.example.com" fstype="cifs" /></pam_mount>
```

## Test mount points <a href="#test-mount-points" id="test-mount-points"></a>

You can now logoff and login again and the mountpoints should be present within the specified mountpoint locations



Source: [https://docs.binarybraids.com/linux/pam\_mount/](https://docs.binarybraids.com/linux/pam_mount/)
