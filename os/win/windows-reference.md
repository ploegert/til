# Windows Reference

### Windows Versions

<table data-full-width="true"><thead><tr><th width="142.13598673300163">Kernal</th><th width="720">Version</th></tr></thead><tbody><tr><td>NT 3.1</td><td>Windows NT 3.1 (All)</td></tr><tr><td>NT 3.5</td><td>Windows NT 3.5 (All)</td></tr><tr><td>NT 3.51</td><td>Windows NT 3.51 (All)</td></tr><tr><td>NT 4.0</td><td>Windows NT 4.0 (All)</td></tr><tr><td>NT 5.0</td><td>Windows 2000 (All)</td></tr><tr><td>NT 5.1</td><td>Windows XP (Home, Pro, MC, Tablet PC, Starter, Embedded)</td></tr><tr><td>NT 5.2</td><td><ul><li>Windows XP (64-bit, Pro 64-bit)</li><li>Windows Server 2003 &#x26; R2 (Standard, Enterprise)</li><li>Windows Home Server</li></ul></td></tr><tr><td>NT 6.0 </td><td><ul><li>Windows Vista (Starter, Home, Basic, Home Premium, Business, Enterprise, Ultimate)</li><li>Windows Server 2008 (Foundation, Standard, Enterprise)</li></ul></td></tr><tr><td>NT 6.1</td><td><ul><li>Windows ~ (Starter, Home, Pro, Enterprise, Ultimate)</li><li>Windows Server 2008 R2 (Foundation, Standard, Enterprise)</li></ul></td></tr><tr><td>NT 6.2</td><td><ul><li>Windows 8 (x86/64, Pro, Enterprise, Windows RT (ARM))</li><li>Windows Phone 8</li><li>Windows Server 2012 (Foundation, Essentials, Standard)</li></ul></td></tr></tbody></table>



### Windows FIles



<table data-full-width="true"><thead><tr><th width="496.9585666293393">Command</th><th width="514">Description</th></tr></thead><tbody><tr><td>%SYSTEMROOT%</td><td>Typically C:\Windows</td></tr><tr><td>%SYSTEMROOT%\System32\drivers\etc\hosts</td><td>DNS entries</td></tr><tr><td>%SYSTEMROOT%\System32\drivers\etc\networks</td><td>Network settings</td></tr><tr><td>%SYSTEt~ROOT% \ system32 \ config\SAM</td><td>User &#x26; password hashes</td></tr><tr><td>%SYSTEMROOT%\repair\SAM</td><td>Backup copy of SAt~</td></tr><tr><td>%SYSTEMROOT%\System32\config\RegBack\SAM</td><td>Backup copy of SAt~</td></tr><tr><td>%WINDIR%\system32\config\AppEvent.Evt</td><td>Application Log</td></tr><tr><td>%WINDIR%\system32\config\SecEvent.Evt</td><td>Security Log</td></tr><tr><td>%ALLUSERSPROFILE%\Start Menu\Programs\Startup\</td><td>Startup Location</td></tr><tr><td>%USERPROFILE%\Start Menu\Programs\Startup\</td><td>Startup Location</td></tr><tr><td>%SYSTEMROOT%\Prefetch</td><td>Prefetch dir (EXE logs)</td></tr></tbody></table>



### Windows System Info Commands

<table data-full-width="true"><thead><tr><th width="449">Command</th><th width="514">Description</th></tr></thead><tbody><tr><td>ver</td><td>Get OS version</td></tr><tr><td>sc query state=all</td><td>Show services</td></tr><tr><td>tasklist /svc</td><td>Show processes &#x26; services</td></tr><tr><td>tasklist /m</td><td>Show all processes &#x26; DLLs</td></tr><tr><td>tasklist /S ip /v</td><td>Remote process listing</td></tr><tr><td>taskkill /PID pid /F</td><td>Force process to terminate</td></tr><tr><td>systeminfo /S ip /U domain\user /P Pwd</td><td>Remote system info</td></tr><tr><td>reg query\\ ip \ RegDomain \ Key /v</td><td>Query remote registry,</td></tr><tr><td>Value</td><td>/s=all values</td></tr><tr><td>reg query HKLM /f password /t REG SZ /s</td><td>Search registry for password</td></tr><tr><td>fsutil fsinfo drives -</td><td>List drives (must be admin)</td></tr><tr><td>dir /a /s /b c:\'.pdf'</td><td>Search for all PDFs</td></tr><tr><td>dir /a /b c:\windows\kb'</td><td>Search for patches</td></tr><tr><td>findstr /si password' .txt I •.xmll •.xls</td><td>Search files for password</td></tr><tr><td>tree /F /A c:\ tree.txt</td><td>Directory listing of C:</td></tr><tr><td>reg save HKLl~\Security security.hive</td><td>Save security hive to file</td></tr><tr><td>echo %USERNAl~E%</td><td>Current user</td></tr></tbody></table>



### Windows System NET/Domain Commands

<table data-full-width="true"><thead><tr><th width="379">Command</th><th width="514">Description</th></tr></thead><tbody><tr><td>net view /domain</td><td>Hosts in current domain</td></tr><tr><td>net view /domain: [t~YDOHAIN]</td><td>Hosts in [l~YDOl1AIN]</td></tr><tr><td>net user /domain</td><td>All users in current domain</td></tr><tr><td>net user user pass /add</td><td>Add user</td></tr><tr><td>net localgroup "Administrators" user /add</td><td>Add user to Administrators</td></tr><tr><td>net accounts /domain</td><td>Domain password policy</td></tr><tr><td>net localgroup "Administrators"</td><td>List local Admins</td></tr><tr><td>net group /domain</td><td>List domain groups</td></tr><tr><td>net group "Domain Adrnins" /domain</td><td>List users in Domain Adrnins</td></tr><tr><td>net group "Domain Controllers 11 /domain</td><td>List DCs for current domain</td></tr><tr><td>net share</td><td>Current SMB shares</td></tr><tr><td>net session I find I "\\"</td><td>Active SHB sessions</td></tr><tr><td>net user user /ACTIVE:jes /domain</td><td>Unlock domain user account</td></tr><tr><td>net user user '' newpassword '' /domain</td><td>Change domain user password</td></tr><tr><td>net share share c:\share /GRANT:Everyone,FULL</td><td>Share folder</td></tr></tbody></table>

### ## Windows Remote Commands

<table data-full-width="true"><thead><tr><th width="438">Command</th><th width="514">Description</th></tr></thead><tbody><tr><td>tasklist /S ip /v</td><td>Remote process listing</td></tr><tr><td>systeminfo /S ip /U domain\user /P Pwd</td><td>Remote systeminfo</td></tr><tr><td>net share \\ ip</td><td>Shares of remote computer</td></tr><tr><td>net use \\ ip</td><td>Remote filesystem (IPC$)</td></tr><tr><td>net use z: \\ ip \share password /user: D0l1AIN\ user</td><td>Map drive, specified credentials</td></tr><tr><td>reg add \\ ip \ regkej \ value</td><td>Add registry key remotely</td></tr><tr><td>sc \\ ip create service binpath=C:\Windows\System32\x.exe start=auto</td><td>Create a remote service (space after start=)</td></tr><tr><td>xcopy /s \\ ip \dir C:\local</td><td>Copy remote folder</td></tr><tr><td>shutdown /m \\ ip /r /t 0 /f</td><td>Remotely reboot machine</td></tr></tbody></table>





### Windows Network Commands

<table data-full-width="true"><thead><tr><th width="438">command</th><th width="514">Description</th></tr></thead><tbody><tr><td>type file</td><td>Display file contents</td></tr><tr><td>del path\' .• /a /s /q /f</td><td>Forceably delete all files in path</td></tr><tr><td>file</td><td>Find "str"</td></tr><tr><td>find /I ''str'' filename</td><td>Line count of</td></tr><tr><td>command I find /c /v</td><td>Schedule file</td></tr><tr><td>at HH:Ml1 file [args]<br>(i.e. at 14:45 cmd /c)</td><td>cmd output to run</td></tr><tr><td>runas /user: user " file [args] 11</td><td>Run file as user</td></tr><tr><td>restart /r /t 0</td><td>Restart now</td></tr><tr><td>tr -d '\15\32' win.txt unix.txt</td><td>Removes CR &#x26; 'Z ('nix)</td></tr><tr><td>makecab file</td><td>Native compression</td></tr><tr><td>Wusa.exe /uninstall /kb: ###</td><td>Uninstall patch</td></tr><tr><td>cmd.exe "wevtutil qe Application /c:40 /f:text /rd:true"</td><td>CLI Event Viewer</td></tr><tr><td>lusrrngr.rnsc</td><td>Local user manager</td></tr><tr><td>services.msc</td><td>Services control panel</td></tr><tr><td>taskmgr.exe</td><td>Task manager</td></tr><tr><td>secpool.rnsc</td><td>Security policy</td></tr><tr><td>eventvwr.rnsc</td><td>Event viewer</td></tr></tbody></table>



### Windows Utility Commands





### Wire Shark Display Filters



{% tabs %}
{% tab title="Ethernet" %}


<table data-header-hidden><thead><tr><th width="172.8318890814558">eth.addr</th><th width="172">eth.len</th><th width="199">eth.src</th></tr></thead><tbody><tr><td>eth.dst</td><td>eth.lg</td><td>eth.trailer</td></tr><tr><td>eth.ig</td><td>eth.multicast</td><td>eth.type</td></tr></tbody></table>
{% endtab %}

{% tab title="IPv4" %}


<table data-header-hidden><thead><tr><th width="333">ip.addr</th><th width="172">ip.fragment.overlap.conflict</th></tr></thead><tbody><tr><td>ip.checksum</td><td>ip.fragment.toolongfragment</td></tr><tr><td>ip.checksum_bad</td><td>ip.fragments</td></tr><tr><td>ip.checksum_good</td><td>ip.hdr_len</td></tr><tr><td>ip.dsfield</td><td>ip.host</td></tr><tr><td>ip.dsfield.ce</td><td>ip.id</td></tr><tr><td>ip.dsfield.dscp</td><td>ip.len</td></tr><tr><td>ip.dsfield.ect</td><td>ip.proto</td></tr><tr><td>ip.dst</td><td>ip.reassembled_in</td></tr><tr><td>ip.dst_host</td><td>ip.src</td></tr><tr><td>ip.flags</td><td>ip.src_host</td></tr><tr><td>ip.flags.df</td><td>ip.tos</td></tr><tr><td>ip.flags.mf</td><td>ip.tos.cost</td></tr><tr><td>ip.flags.rb</td><td>ip.tos.delay</td></tr><tr><td>ip.frag_offset</td><td>ip.tos.precedence</td></tr><tr><td>ip.fragment</td><td>ip.tos.reliability</td></tr><tr><td>ip.fragment.error</td><td>ip.tos.throughput</td></tr><tr><td>ip.fragment.multipletails</td><td>ip.ttl</td></tr><tr><td>ip.fragment.overlap</td><td>ip.version</td></tr></tbody></table>
{% endtab %}

{% tab title="IPv6 " %}


<table data-header-hidden><thead><tr><th width="292.2653465346534">ipv6.addr</th><th width="388">ipv6.hop_opt</th></tr></thead><tbody><tr><td>ipv6.class</td><td>ipv6.host</td></tr><tr><td>ipv6.dst</td><td>ipv6.mipv6_home_address</td></tr><tr><td>ipv6.dst_host</td><td>ipv6.mipv6_length</td></tr><tr><td>ipv6.dst_opt</td><td>ipv6.mipv6_type</td></tr><tr><td>ipv6.flow</td><td>ipv6.nxt</td></tr><tr><td>ipv6.fragment</td><td>ipv6.opt.pad1</td></tr><tr><td>ipv6.fragment.error</td><td>ipv6.opt.padn</td></tr><tr><td>ipv6.fragment.more</td><td>ipv6.plen</td></tr><tr><td>ipv6.fragment.multipletails</td><td>ipv6.reassembled_in</td></tr><tr><td>ipv6.fragment.offset</td><td>ipv6.routing_hdr</td></tr><tr><td>ipv6.fragment.overlap</td><td>ipv6.routing_hdr.addr</td></tr><tr><td>ipv6.fragment.overlap.conflict</td><td>ipv6.routing_hdr.left</td></tr><tr><td>ipv6.fragment.toolongfragment</td><td>ipv6.routing_hdr.type</td></tr><tr><td>ipv6.fragments</td><td>ipv6.src</td></tr><tr><td>ipv6.fragment.id</td><td>ipv6.src_host</td></tr><tr><td>ipv6.hlim</td><td>ipv6.version</td></tr></tbody></table>
{% endtab %}

{% tab title="IEEE 802.1Q" %}
<table><thead><tr><th width="333">vlan.cfi</th><th width="172">vlan.id</th><th width="72">vlan.priority</th></tr></thead><tbody><tr><td>vlan.etype</td><td>vlan.len</td><td>vlan.trailer</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

