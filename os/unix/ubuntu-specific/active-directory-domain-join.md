# Active Directory Domain Join

This guide assumes you have an existing Active Directory domain. This guide also assumes Debian based distributions.

### Install required packages <a href="#install-required-packages" id="install-required-packages"></a>

```
sudo apt install sssd-ad sssd-tools realmd adcli
```

### Confirm domain discovery via DNS <a href="#confirm-domain-discovery-via-dns" id="confirm-domain-discovery-via-dns"></a>

```
sudo realm -v discover ad1.example.com
```

You should see a result similar to the following if successful

```
 * Resolving: _ldap._tcp.ad1.example.com * Performing LDAP DSE lookup on: 10.51.0.5 * Successfully discovered: ad1.example.comad1.example.com  type: kerberos  realm-name: AD1.EXAMPLE.COM  domain-name: ad1.example.com  configured: no  server-software: active-directory  client-software: sssd  required-package: sssd-tools  required-package: sssd  required-package: libnss-sss  required-package: libpam-sss  required-package: adcli  required-package: samba-common-bin
```

### Join device to domain <a href="#join-device-to-domain" id="join-device-to-domain"></a>

Note

This `adcli` command is being used for domain join. This is to combat issues with Server 2025 Domain Controllers as per: [https://gitlab.freedesktop.org/realmd/adcli/-/issues/40](https://gitlab.freedesktop.org/realmd/adcli/-/issues/40)

```
adcli join -U yout_user@YOUR.REALM --domain-controller=your.dc.fqdn --verbose --ldap-passwd
```

### Configure SSSD <a href="#configure-sssd" id="configure-sssd"></a>

If the domain join operation was successful create a default SSSD configuration file at `/etc/sssd/sssd.conf` and make sure to `chmod 600` on the file once configured.

```
[sssd]domains = ad1.example.comconfig_file_version = 2services = nss, pam[domain/ad1.example.com]default_shell = /bin/bashkrb5_store_password_if_offline = Truecache_credentials = Truekrb5_realm = AD1.EXAMPLE.COMrealmd_tags = manages-system joined-with-adcliid_provider = adfallback_homedir = /home/%u@%dad_domain = ad1.example.comuse_fully_qualified_names = Trueldap_id_mapping = Trueaccess_provider = ad
```

Restart the SSSD service for configuration settings to be applied.

```
sudo systemctl restart sssd
```

### Allow user home directory creation <a href="#allow-user-home-directory-creation" id="allow-user-home-directory-creation"></a>

This will allow any AD users to automatically create a new home directory upon logon.

```
sudo pam-auth-update --enable mkhomedir
```

### Testing setup <a href="#testing-setup" id="testing-setup"></a>

#### Fetch AD User information <a href="#fetch-a-d-user-information" id="fetch-a-d-user-information"></a>

```
$ getent passwd john@ad1.example.comjohn@ad1.example.com:*:1725801106:1725800513:John Smith:/home/john@ad1.example.com:/bin/bash
```

#### Fetch AD Group membership information <a href="#fetch-a-d-group-membership-information" id="fetch-a-d-group-membership-information"></a>

```
$ groups john@ad1.example.comjohn@ad1.example.com : domain users@ad1.example.com engineering@ad1.example.com
```

Source: [https://docs.binarybraids.com/linux/ad\_domain\_join/](https://docs.binarybraids.com/linux/ad_domain_join/)
