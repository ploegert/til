---
description: 'Source: https://github.com/henk52/knowledgesharing/wiki/FedoraCookbook'
---

# Fedora

## Fedora wiki

### Introduction

#### References

* http://www.devopsbookmarks.com/orchestration
* https://access.redhat.com/documentation/en-US/Red\_Hat\_Enterprise\_Linux/7/html/Networking\_Guide/sec-Understanding\_the\_Device\_Renaming\_Procedure.html
* Installing from USB
  * http://www.softpanorama.org/Commercial\_linuxes/RHEL/Installation/installation\_from\_usb\_drive.shtml
  * http://slashsarc.com/2013/12/make-a-rhel-6-bootable-usb-installer/
  * https://access.redhat.com/documentation/en-US/Red\_Hat\_Enterprise\_Linux/7/html/Installation\_Guide/sect-making-usb-media.html
  * https://blog.netnerds.net/2006/04/ribcl-reset-administrator-password-on-ilo/
* Upgrade: https://fedoraproject.org/wiki/DNF\_system\_upgrade
* IPMI
  * https://www.thomas-krenn.com/en/wiki/Configuring\_IPMI\_under\_Linux\_using\_ipmitool
  * http://russell.ballestrini.net/how-to-reset-hp-ilo-lights-out-user-and-password-settings-with-ipmitools/

### Background info

#### Kernel

* http://www.tldp.org/LDP/Linux-Filesystem-Hierarchy/html/proc.html
* https://github.com/torvalds/linux
  * ./Documentation/sysctl/kernel.txt
  * ./include/uapi/linux/shm.h
* shmmax Parameter: the maximum size in bytes of a single shared memory segment that a Linux process can allocate in its virtual address space.
  * Default: (ULONG\_MAX - (1UL << 24))
    * include/uapi/linux/shm.h
  * It also seems to be the max total amount of shared memory a process can attach to.
    * So if SHMMAX is 3G and 3 process has allocated 2GB each, then a process can only attach to one of the pages at a time. (from RHEL3?)
  * Access to it:
    * cat /proc/sys/kernel/shmmax
    * sysctl -w kernel.shmmax=2147483648
    * echo "kernel.shmmax=2147483648" >> /etc/sysctl.conf
* shmall: The total amount of shared memory pages that can be used system wide.
  * Please note this is in pages, not in bytes.
* getconf PAGE\_SIZE
* ipcs -m
* pmap -p PID
* /proc/PID/map
  * containing the currently mapped memory regions and their access permissions.
  * The format is:
    * address perms offset dev inode pathname
  * From: Documentation/filesystems/proc.txt in the linux source tree.
  * I looks like the "inode" for shm is actually the shmid (not the key)
    * it seems that all the /SYSV are shared memory.

## Cook Book

#### DNF

* dnf clean all
* Disable Delta RPMs
  * vi /etc/dnf/dnf.conf

```
# disable delta RPMs
deltarpm=0
```

#### Network

**tcpdump**

* http://www.cyberciti.biz/faq/tcpdump-capture-record-protocols-port/
* https://danielmiessler.com/study/tcpdump/#source-destination
* tcpdump -i eth0 dst 192.168.1.18

**Adding static assignments in dnsmaq**

See: http://docs.slackware.com/howtos:network\_services:dhcp\_server\_via\_dnsmasq

**Default GW**

* https://access.redhat.com/documentation/en-US/Red\_Hat\_Enterprise\_Linux/6/html/Deployment\_Guide/s1-networkscripts-static-routes.html

**Domain name**

* https://access.redhat.com/solutions/1276563

**Find Out What Program / Service is Listening on a Specific TCP Port**

* http://www.cyberciti.biz/faq/find-out-which-service-listening-specific-port/
* lsof -Pnl +M -i4

#### Routing

* https://linuxconfig.org/how-to-turn-on-off-ip-forwarding-in-linux
* https://docs.fedoraproject.org/en-US/Fedora/13/html/Security\_Guide/sect-Security\_Guide-Firewalls-FORWARD\_and\_NAT\_Rules.html

#### IP Tables

* https://www.frozentux.net/iptables-tutorial/chunkyhtml/c962.html
* https://www.karlrupp.net/en/computer/nat\_tutorial
* https://serverfault.com/questions/233760/port-forwarding-from-host-to-guest-with-libvirt-0-8-3-using-kvm-on-ubuntu
* https://docs.fedoraproject.org/en-US/Fedora/13/html/Security\_Guide/sect-Security\_Guide-Firewalls-FORWARD\_and\_NAT\_Rules.html
* https://www.cyberciti.biz/faq/howto-iptables-show-nat-rules/
* https://access.redhat.com/documentation/en-US/Red\_Hat\_Enterprise\_Linux/4/html/Security\_Guide/s1-firewall-ipt-fwd.html
* https://fedoraproject.org/wiki/How\_to\_edit\_iptables\_rules#Making\_changes\_persistent
  * /etc/sysconfig/iptables
* commands
  * iptables
  * iptables-save
    * iptables-save > /etc/sysconfig/iptables
* iptables -A PREROUTING -t nat -i eth0-p tcp --dport 443 -j DNAT --to 192.168.41.6:443
  * Add
* iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 443 -j DNAT --to 192.168.41.6:443
  * Delete
* iptables -L -t nat -n
  * List NAT table with numbers
* iptables -t nat -F
  * Clear the nat table entries.

**Port forwarding to sub nets**

* Source: https://access.redhat.com/documentation/en-US/Red\_Hat\_Enterprise\_Linux/4/html/Security\_Guide/s1-firewall-ipt-fwd.html
* Manual set-up
  * iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
  * iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 1022 -j DNAT --to 10.1.2.3:22
  * iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 2022 -j DNAT --to 10.2.3.4:22
* Permanent storage
  * Source https://access.redhat.com/documentation/en-US/Red\_Hat\_Enterprise\_Linux/4/html/Security\_Guide/s2-firewall-ipt-act-sav.html
  * RHEL6: /sbin/service iptables save
  * Stored in: /etc/sysconfig/iptables
* For KVMs
* https://serverfault.com/questions/233760/port-forwarding-from-host-to-guest-with-libvirt-0-8-3-using-kvm-on-ubuntu

**Firewall**

* https://www.liquidweb.com/kb/how-to-stop-and-disable-firewalld-on-fedora-21/
* sudo firewall-cmd --add-service=cockpit --permanent
* sudo firewall-cmd --reload

#### SFTP service

**Enable user as sftp account, for remote edit**

See: https://www.server-world.info/en/note?os=Fedora\_25\&p=ssh\&f=5

1. groupadd sftp\_users
2. usermod -G sftp\_users fedora
3. vi /etc/ssh/sshd\_config

* It seems that after the ssh\_config update the users in sftp\_users can only use sftp, not ssh
* Also you can't put another users home dir in the ChrootDirectory you get a broken link.

4. Subsystem sftp /usr/libexec/openssh/sftp-server Subsystem sftp internal-sftp
5. add to the end

```
Match Group sftp_users
  X11Forwarding no
  AllowTcpForwarding no
  ChrootDirectory /home
  ForceCommand internal-sftp
```

#### devices

**Detecting if there is a CDROM in the drive**

See: http://superuser.com/questions/630588/how-to-detect-whether-there-is-a-cd-rom-in-the-drive

You can get information about any block device using the command blkid.

```
[root@arch32-vm ~]# blkid /dev/sr0
/dev/sr0: UUID="2013-05-31-23-04-19-00" LABEL="ARCH_201306" TYPE="iso9660" PTTYPE="dos"
[root@arch32-vm ~]# echo $?
0
If I remove the disk, I don't get any output and exit value is 2. (0 means success. A non-zero value will typically mean something abnormal happen or an error occurred)

[root@arch32-vm ~]# blkid /dev/sr0
[root@arch32-vm ~]# echo $?
2
```

**Fixing slow ssh password prompt**

See: http://www.doublecloud.org/2013/06/slow-ssh-client-and-quick-hack/ and: http://askubuntu.com/questions/246323/why-does-sshs-password-prompt-take-so-long-to-appear

Further searching got me a page that suggests to use –o switch in the ssh command as follows.

1. ssh -o GSSAPIAuthentication=no root@192.168.98.155

The result is instant response for password, so the problem was solved. But I could not change the command line called by PackStack, so I had to make the change default without the switch.

To change it system wide, you can change the file in /etc/ssh folder as follows:

2. vim /etc/ssh/ssh\_config

Host \*

GSSAPIAuthentication no It’s also possible to change it just for a particular user – just change the file “config” under the hidden folder .ssh of the user’s home directory. For example, you can change it using the following command for root user.

3. vim /root/.ssh/config

Skipping GSSAPIAuthentication may have some impact on security. To find out more, check out the wiki page here.

**Enable ssh access to machine, without the use of a password**

From: https://okeanos.grnet.gr/support/faq/cyclades-how-can-i-add-my-public-ssh-key-in-an-existing-vm/

Generate the key pair

1. ssh-keygen -t rsa -f autolab
2. Then add the public key(including the second entry the '@' otherwise it wont work).

```
mkdir -p ~/.ssh
echo <public key contents> >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
```

**ssh in to temporary VM without having to answer yes to adding the host key**

See: http://askubuntu.com/questions/246323/why-does-sshs-password-prompt-take-so-long-to-appear

ssh -i autolab -o StrictHostKeyChecking=no 192.168.122.229

### systemd

* https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files
* https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units

#### Systemctl

* https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files
* https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units
* https://access.redhat.com/documentation/en/red-hat-enterprise-linux/?version=7/ system administration book.
* https://fedoramagazine.org/systemd-unit-dependencies-and-order/

Preset

* https://freedesktop.org/wiki/Software/systemd/Preset/
* systemctl daemon-reload
  * Always run the systemctl d aemo n-rel o ad command after creating new unit files or modifying existing unit files
* systemctl list-dependencies --after gdm.service
  * what services are ordered to start before the specified service
* systemctl list-dependencies --before gdm.service
  * what services are ordered to start after the specified service
* systemctl enable name.service
  * reads the \[Install] section of the selected service unit and creates appropriate symbolic links to the /usr/lib/systemd/system/name.service file in the /etc/systemd /system/ directory and its subdirectories. (Does not re-create an existing link).
* systemctl reenable name.service
* systemctl disable name.service
* systemctl mask name.service
  * to prevent it from being started manually or by another service.
* systemctl unmask name.service
* systemctl list-units --type target
* systemctl get-default
* systemctl list-units --type target \[--all]
* systemctl show name.service \[-p PARM]
* systemd -delta
* Target units

**Content of the unit.service file**

http://www.dsm.fordham.edu/cgi-bin/man-cgi.pl?topic=systemd.unit\&ampsect=5

* \[Unit]
  * After: Start this unit(defined by this file) to start after the units give in the after list.
    * What happens if a unit in the 'After=' list isn't started?
  * Before: Start this unit, before the given list of units.
  * Requires: Dont start this unit until the given unit is running, start the given unit if needed.
    * If the given unit doesn't start, then this unit isn't started.
    * If one of the other units gets deactivated or its activation fails, this unit will be deactivated.
    * If the given unit is restarted, then this unit is restarted as well(I think)
    * Note that requirement dependencies do not influence the order in which services are started or stopped. This has to be configured independently with the After= or Before= options.
  * Wants: start this unit if the given unit is running or attempted to be started.
    * If the given unit doesn't start successfully, this unit is still started.

**xorp example**

1. usr/bin/systemctl preset xorp.service
2. /usr/bin/systemctl enable xorp.service
3. /usr/bin/systemctl start xorp

See also: http://0pointer.de/public/systemd-man/systemd.unit.html

```
[Unit]
Description=XORP softrouter.
After=network.target

[Service]
Type=forking
PIDFile=/var/run/xorp.pid
ExecStart=/root/xorp_bg
#ExecReload=/opt/sonar/bin/linux-x86-64/sonar.sh restart
ExecStop=/root/stop_xorp

Restart=always

[Install]
WantedBy=multi-user.target
```

### Services

**Enable rsh**

* http://people.redhat.com/kzak/docs/rsh-rlogin-howto.html

#### NTP

* http://www.satsignal.eu/ntp/Raspberry-Pi-quickstart.html
* http://askubuntu.com/questions/429306/ntpdate-no-server-suitable-for-synchronization-found
* https://bugzilla.redhat.com/show\_bug.cgi?id=1255098

**NTP server**

```
package { 'ntp': ensure => present, }

# ntpq -pn


# Disable selinux
# /etc/selinux/config

# disable the firewalld
service { 'firewalld':
  ensure => stopped,
  enable => false,
}

$szNicName="ens33"
$szServiceIpAddress="10.1.2.8"
network::if::static { "$szNicName": ensure => 'up', ipaddress => "$szServiceIpAddress", netmask => '255.255.255.0', }

# http://www.thegeekstuff.com/2014/06/linux-ntp-server-client/
# noquery prevents dumping status data from ntpd.
# notrap prevents control message trap service.
# nomodify prevents all ntpq queries that attempts to modify the server.
# nopeer prevents all packets that attempts to establish a peer association.
# Kod - Kiss-o-death packet is to be sent to reduce unwanted querie
file_line { 'ntp_conf_restrict':
  path => '/etc/ntp.conf',
  line => 'restrict default kod nomodify notrap nopeer noquery',
  match => '^restrict default',
}


file_line { 'ntp_conf_restrict_local':
  path => '/etc/ntp.conf',
  line => 'restrict 10.1.2.0 mask 255.255.255.0 nomodify notrap',
}


file_line { 'ntp_conf_server':
  path => '/etc/ntp.conf',
  line => 'server  127.127.1.0 # local clock',
  match => '^server 192.168',
}

file_line { 'ntp_conf_fudge':
  path => '/etc/ntp.conf',
  line => 'fudge   127.127.1.0 stratum 1',
}

file_line { 'ntp_conf_log':
  path => '/etc/ntp.conf',
  line => 'logfile /var/log/ntp.log',
}


service { 'ntpd':
  ensure  => running,
  enable  => true,
  require => File_line['ntp_conf_restrict','ntp_conf_server','ntp_conf_fudge','ntp_conf_log','ntp_conf_restrict_local'],
}

```

* ntpq -pn

```
ntpq -pn
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
 127.127.1.0     .LOCL.           1 l    -   64    0    0.000    0.000   0.000
```

**NTP client**

* Please note: If the 'receive(10.1.2.8)' is not seen, the possibly there is a firewall between the client and the server.

/usr/sbin/ntpdate -v -d -U ntp -b -p 2 10.1.2.8

```
 X May 10:49:36 ntpdate[XXX]: ntpdate 4.XXX@XXX XXXXXXXXX (1)
Looking for host 10.1.2.8 and service ntp
host found : 10.1.2.8
transmit(10.1.2.8)
receive(10.1.2.8)
transmit(10.1.2.8)
receive(10.1.2.8)
server 10.1.2.8, port 123
stratum 2, precision -24, leap 00, trust 000
refid [10.1.2.8], delay 0.02579, dispersion 0.00000
transmitted 2, in filter 2
reference time:    dadae999.21e6f516  Mon, May  9 2016 10:48:57.132
originate timestamp: dadae9c2.c68890b3  Mon, May  9 2016 10:49:38.775
transmit timestamp:  dadae9c2.cc6bfa06  Mon, May  9 2016 10:49:38.798
filter delay:  0.02586  0.02579  0.00000  0.00000 
         0.00000  0.00000  0.00000  0.00000 
filter offset: -0.02313 -0.02315 0.000000 0.000000
         0.000000 0.000000 0.000000 0.000000
delay 0.02579, dispersion 0.00000
offset -0.023155

 9 May 10:49:38 ntpdate[XXX]: step time server 10.1.233.88 offset -0.023155 sec
```

#### writing your own systemd services

**Writing a oneshot systemd service**

```
[Unit]
Description=execute /usr/local/bin/autoinstfromisodev.sh at start-up.
After=local-fs.target

[Service]
Type=oneshot
ExecStart=/usr/local/bin/autoinstfromisodev.sh
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```

### Applications

#### Installing Mono

* https://blog.kloud.com.au/2016/05/30/installing-mono-into-amazon-linux/
* http://www.mono-project.com/download/#download-lin-centos

### Installation

#### Create USB for booting

1. Download the decired ISO from [Fedora project](https://getfedora.org/)
2. Write the .iso to the USB using gnome disk [as described](https://itsfoss.com/create-fedora-live-usb-ubuntu/)

There are different collections at: [Fedora spins](https://spins.fedoraproject.org/)

and older .iso versions can also be found e.g. [Fedora 37 ISOs](https://download.fedoraproject.org/pub/fedora/linux/releases/37/Spins/x86_64/iso/)

#### Installation on bare metal

#### Installation in VM

#### Upgrade

* https://fedoramagazine.org/upgrading-fedora-31-to-fedora-32/
* https://unix.stackexchange.com/questions/579184/upgrade-from-fedora-30-to-31-cannot-enable-multiple-streams-for-module-ant
* I has to disable the vscode repo, otherwise it kept failing

1. sudo dnf upgrade --refresh
2. sudo dnf install dnf-plugin-system-upgrade
3. sudo dnf system-upgrade download --releasever=32
4. sudo dnf system-upgrade reboot

### Trouble shooting

#### troubleshooting Installation

**F23 fail cp dracut**

Turns out it needs 1GB RAM, and then it works.

* https://fedorahosted.org/fedora-infrastructure/ticket/4930

#### Troubleshooting Services

**ntpdate: no server suitable for synchronization found**

Update the server IP addresses in both

* /etc/ntp.conf
* /etc/ntp/step-tickers

#### X

**xterm Xt error: Can't open display: %s**

Answer: hostname and the hostname in /etc/sysconfig/network had to be the same. the VNC server used the name from: /etc/sysconfig/network

Changed the HOSTNAME= in /etc/sysconfig/network and restarted vncserver and it worked.

```
:No protocol specified
Warning: This program is an suid-root program or is being run by the root user.
The full text of the error or warning message cannot be safely formatted
in this environment. You may get a more descriptive message by running the
program as a non-root user or by removing the suid bit on the executable.
xterm Xt error: Can't open display: %s
```
