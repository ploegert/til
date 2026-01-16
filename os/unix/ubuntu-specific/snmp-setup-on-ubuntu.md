# SNMP Setup on Ubuntu

This guide will showcase installing SNMP on an Ubuntu system.

### Update system packages <a href="#update-system-packages" id="update-system-packages"></a>

`sudo apt-get update && sudo apt-get upgrade`

### Install SNMP package <a href="#install-snmp-package" id="install-snmp-package"></a>

`sudo apt-get install snmpd`

### Configure agent listener <a href="#configure-agent-listener" id="configure-agent-listener"></a>

Open `/etc/snmp/snmpd.conf` in the editor of your choice and edit the `agentAddress` section. Also remove the `#` in front for it to take effect

Listen on all addresses

```
agentAddress udp:161,udp6:[::1]:161
```

Listen on specific address

```
agentAddress udp:192.168.100.2:161
```

### Configure community string <a href="#configure-community-string" id="configure-community-string"></a>

Open `/etc/snmp/snmpd.conf` in the editor of your choice and edit the `rocommunity` section.

set `public` to an alternative community name if needed

### Restart SNMP Service <a href="#restart-snmp-service" id="restart-snmp-service"></a>

```
sudo service snmpd restart
```



Source: [https://docs.binarybraids.com/linux/snmp\_config/](https://docs.binarybraids.com/linux/snmp_config/)



