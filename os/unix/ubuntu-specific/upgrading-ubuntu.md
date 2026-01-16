# Upgrading Ubuntu

I recently discovered that my Linode box was running a fairly old version of Ubuntu. Because it is a remote box that I SSH into, there is no graphical user interface. Upgrading to a newer release can be accomplished with the following command line utility:

```
$ do-release-upgrade
```

It includes a series of prompts regarding choices about the upgrade and a lot of waiting.

Adding the `-d` flag will upgrade to the latest development release.



## Ubuntu Distribution Upgrade <a href="#ubuntu-distribution-upgrade" id="ubuntu-distribution-upgrade"></a>

Upgrading Ubuntu from one LTS version to the next is a fairly simple process in principal.

Warning

Please make sure to test this on a non-critical system or take backups prior to upgrading your system!

### Update system packages <a href="#update-system-packages" id="update-system-packages"></a>

```
sudo apt update && sudo apt upgrade 
```

### Install Ubuntu update tool <a href="#install-ubuntu-update-tool" id="install-ubuntu-update-tool"></a>

```
sudo apt install update-manager-core
```

## Run system upgrade <a href="#run-system-upgrade" id="run-system-upgrade"></a>

```
sudo do-release-upgrade
```

## Reboot system <a href="#reboot-system" id="reboot-system"></a>

```
sudo reboot 
```

You can now verify the upgrades and rollback if any issues are encountered.
