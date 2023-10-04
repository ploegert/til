# Reference

### Linux Network Commands

| watch ss -tp                                 | Network connections      |
| -------------------------------------------- | ------------------------ |
| netstat -ant                                 | Tcp connections -anu=udp |
| netstat -tulpn                               | Connections with PIDs    |
| lsof -i                                      | Established connections  |
| smb:// ip /share                             | Access windows smb share |
| share user x.x.x.x c$                        | Mount Windows share      |
| smbclient -0 user\\\\\\\ ip \\\ share        | Sl1B connect             |
| ifconfig eth# ip I cidr                      | Set IP and netmask       |
| ifconfig ethO:l ip I cidr                    | Set virtual interface    |
| route add default gw gw lp                   | Set GW                   |
| ifconfig eth# mtu \[size]                    | Change t\~TO size        |
| export l1AC=xx: XX: XX: XX: XX: XX           | Change t\~AC             |
| ifconfig int hw ether t\~AC                  | Change t\~AC             |
| macchanger -m l1AC int                       | Backtrack t\~AC changer  |
| iwlist int scan                              | Built-in wifi scanner    |
| dig -x ip                                    | Domain lookup for IP     |
| host ip                                      | Domain lookup for IP     |
| host -t SRV service tcp.url.com              | Domain SRV lookup        |
| dig @ ip domain -t AXrR                      | DNS Zone Xfer            |
| host -1 domain namesvr                       | DNS Zone Xfer            |
| ip xfrm state list                           | Print existing VPN kejs  |
| ip addr add ip I cidr aev ethO               | Adds 'hidden' interface  |
| /var/log/messages I grep DHCP                | List DHCP assignments    |
| tcpkill host ip and port port                | Block ip:port            |
| echo "1" /proc/sys/net/ipv4/ip forward       | Turn on IP Forwarding    |
| echo ''nameserver x.x.x.x'' /etc7resolv.conf | Add DNS Server           |

### Linux system Info

<table><thead><tr><th width="297">Command</th><th width="224">Description</th></tr></thead><tbody><tr><td>nbtstat -A ip</td><td>Get hostname for ip</td></tr><tr><td>id</td><td>Current username</td></tr><tr><td>w</td><td>Logged on users</td></tr><tr><td>who -a</td><td>User information</td></tr><tr><td>last -a</td><td>Last users logged on</td></tr><tr><td>ps -ef</td><td>Process listing (top)</td></tr><tr><td>df -h</td><td>Disk usage (free)</td></tr><tr><td>uname -a</td><td>Kernel version/CPU info</td></tr><tr><td>mount</td><td>t1ounted file Sjstems</td></tr><tr><td>getent passwd</td><td>Show list of users</td></tr><tr><td>PATH~$PATH:/home/mypath</td><td>Add to PATH variable</td></tr><tr><td>kill pid</td><td>Kills process with pid</td></tr><tr><td>cat /etc/issue</td><td>Show OS info</td></tr><tr><td>cat /etc/'release'</td><td>Show OS version info</td></tr><tr><td>cat /proc/version</td><td>Show kernel info</td></tr><tr><td>rpm --querJ -all</td><td>Installed pkgs (Redhat)</td></tr><tr><td>rpm -ivh ) .rpm</td><td>Install RPM (-e~remove)</td></tr><tr><td>dpkg -get-selections</td><td>Installed pkgs (Obuntu)</td></tr><tr><td>dpkg -I '.deb</td><td>Install DEB (-r~remove)</td></tr><tr><td>pkginfo</td><td>Installed pkgs (Solaris)</td></tr><tr><td>which tscsh/csh/ksh/bash</td><td>Show location of executable</td></tr><tr><td>chmod -so tcsh/csh/ksh</td><td>Disable shell , force bash</td></tr></tbody></table>

### Linux Utility Commands

<table><thead><tr><th width="297">Command</th><th width="224">Description</th></tr></thead><tbody><tr><td>wget http:// url -0 url.txt -o /dev/null</td><td>Grab url</td></tr><tr><td>rdesktop ip</td><td>Remote Desktop to ip</td></tr><tr><td>scp /tmp/file user@x.x.x.x:/tmp/file</td><td>Put file</td></tr><tr><td>scp user@ remoteip :/tmp/file /tmp/file</td><td>Get file</td></tr><tr><td>useradd -m user</td><td>Add user</td></tr><tr><td>passwd user</td><td>Change user password</td></tr><tr><td>rmuser unarne</td><td>Remove user</td></tr><tr><td>script -a outfile</td><td>Record shell : Ctrl-D stops</td></tr><tr><td>apropos subject</td><td>Find related command</td></tr><tr><td>history</td><td>View users command history</td></tr><tr><td>! num</td><td>Executes line # in history</td></tr></tbody></table>



### Linux File Commands:

<table><thead><tr><th width="379">Command</th><th width="224">Description</th></tr></thead><tbody><tr><td>diff filel file2</td><td>Compare files</td></tr><tr><td>rm -rf dir</td><td>Force delete of dir</td></tr><tr><td>shred -f -u file</td><td>Overwrite/delete file</td></tr><tr><td>touch -r ref file file</td><td>t1atches ref_ file timestamp</td></tr><tr><td>touch -t YYYY11t1DDHHSS file</td><td>Set file timestamp</td></tr><tr><td>sudo fdisk -1</td><td>List connected drives</td></tr><tr><td>mount /dev/sda# /mnt/usbkey</td><td>t1ount USB key</td></tr><tr><td>md5sum -t file</td><td>Compute md5 hash</td></tr><tr><td>echo -n "str11 I md5sum</td><td>Generate md5 hash</td></tr><tr><td>shalsum file</td><td>SHAl hash of file</td></tr><tr><td>sort -u</td><td>Sort/show unique lines</td></tr><tr><td>grep -c ''str'' file</td><td>Count lines w/ ''str''</td></tr><tr><td>tar cf file.tar files</td><td>Create .tar from files</td></tr><tr><td>tar xf file.tar</td><td>Extract .tar</td></tr><tr><td>tar czf file.tar.gz files</td><td>Create .tar.gz</td></tr><tr><td>tar xzf file.tar.gz</td><td>Extract .tar.gz</td></tr><tr><td>tar cjf file.tar.bz2 files</td><td>Create .tar.bz2</td></tr><tr><td>tar xjf file.tar.bz2</td><td>Extract .tar.bz2</td></tr><tr><td>gzip file</td><td>Compress/rename file</td></tr><tr><td>gzip -d file. gz</td><td>Decompress file.gz</td></tr><tr><td>upx -9 -o out.exe orig.exe</td><td>UPX packs orig.exe</td></tr><tr><td>zip -r zipname.zip \Directory\'</td><td>Create zip</td></tr><tr><td>dd skip=lOOO count=2000 bs=S if=file of=file</td><td>Cut block 1K-3K from file</td></tr><tr><td>split -b 9K \ file prefix</td><td>Split file into 9K chunks</td></tr><tr><td>awk 'sub("$"."\r")' unix.txt win.txt</td><td>Win compatible txt file</td></tr><tr><td>find -i -name file -type '.pdf</td><td>Find PDF files</td></tr><tr><td>find I -perm -4000 -o -perm -2000 -exec ls -ldb {) \;</td><td>Search for setuid files</td></tr><tr><td>dos2unix file</td><td>Convert to ~nix format</td></tr><tr><td>file file</td><td>Determine file type/info</td></tr><tr><td>chattr (+/-)i file</td><td>Set/Unset immutable bit</td></tr></tbody></table>



### Linus Misc Commands

<table><thead><tr><th width="379">Command</th><th width="224">Description</th></tr></thead><tbody><tr><td>unset HISTFILE</td><td>Disable history logging</td></tr><tr><td>ssh user@ ip arecord - I aplay -</td><td>Record remote mic</td></tr><tr><td>gee -o outfile myfile.c</td><td>Compile C,C++</td></tr><tr><td>init 6</td><td>Reboot (0 = shutdown)</td></tr><tr><td>cat /etc/ 1 syslog 1 .conf 1 grep -v ''"#''</td><td>List of log files</td></tr><tr><td>grep 'href=' file 1 cut -d"/" -f3 I grep url lsort -u</td><td>Strip links in url.com</td></tr><tr><td>dd if=/dev/urandom of= file bs=3145"28 count=lOO</td><td>Make random 311B file</td></tr></tbody></table>

### Linux File System Structure

<table><thead><tr><th width="379">Command</th><th width="224">Description</th></tr></thead><tbody><tr><td>/bin</td><td>User binaries</td></tr><tr><td>/boot</td><td>Boot-up related files</td></tr><tr><td>/dev</td><td>Interface for system devices</td></tr><tr><td>/etc</td><td>Systern configuration files</td></tr><tr><td>/horne</td><td>Base directory for user files</td></tr><tr><td>/lib</td><td>Critical software libraries</td></tr><tr><td>/opt</td><td>Third party software</td></tr><tr><td>/proc</td><td>Systern and running programs</td></tr><tr><td>/root</td><td>Home directory of root user</td></tr><tr><td>/sbin</td><td>System administrator binaries</td></tr><tr><td>/trnp</td><td>Temporary files</td></tr><tr><td>/usr</td><td>Less critical files</td></tr><tr><td>/var</td><td>Variable Systern files</td></tr></tbody></table>



### Linux Files

<table><thead><tr><th width="379">Command</th><th width="224">Description</th></tr></thead><tbody><tr><td>/etc/shadow</td><td>Local users' hashes</td></tr><tr><td>/etc/passwd</td><td>Local users</td></tr><tr><td>/etc/group</td><td>Local groups</td></tr><tr><td>/etc/rc.d</td><td>Startup services</td></tr><tr><td>/etc/init.d</td><td>Service</td></tr><tr><td>/etc/hosts</td><td>Known hostnames and IPs</td></tr><tr><td>/etc/HOSTNAl1E</td><td>Full hostnarne with domain</td></tr><tr><td>/etc/network/interfaces</td><td>Network configuration</td></tr><tr><td>/etc/profile</td><td>System environment variables</td></tr><tr><td>/etc/apt/sources.list</td><td>Ubuntu sources list</td></tr><tr><td>/etc/resolv.conf</td><td>Narneserver configuration</td></tr><tr><td>/horne/ user /.bash historj</td><td>Bash history (also /root/)</td></tr><tr><td>/usr/share/wireshark/rnanuf</td><td>Vendor-t1AC lookup</td></tr><tr><td>-/.ssh/</td><td>SSH keystore</td></tr><tr><td>/var/log</td><td>System log files (most Linux)</td></tr><tr><td>/var/adrn</td><td>System log files (Unix)</td></tr><tr><td>/var/spool/cron</td><td>List cron files</td></tr><tr><td>/var/log/apache/access.log</td><td>Apache connection log</td></tr><tr><td>/etc/fstab</td><td>Static file system info</td></tr></tbody></table>
