# Lab Set Up (not on exam)

## Installing LDAP & Integrated DNS Server w/ FreeIPA Server Solution (RHEL 8)

---

Commands:
- `yum install -y @idm:DL1` - to install identity management system module [`@idm` = module; `DL1` = stream]
- `yum install -y freeipa-server bind-dyndb-ldap ipa-server-dns` - to install all required packages
- `ipa-server-install` - to install FreeIPA server
> - Do you want to configure integrated DNS (BIND)? [no]: **yes**
> - Server host name [ipaserver.example.com]: **ipaserver.example.com**
> - Please confirm the domain name [example.com]: **example.com**
> - Please provide a realm name [EXAMPLE.COM]: **EXAMPLE.COM**
> - Directory Manager password: *********
> - Password (confirm): *********
> - IPA admin password: *********
> - Password (confirm): *********
> - Do you want to configure DNS forwarders? [yes]: **no**
> - Do you want to search for missing the reverse zone? [yes]: **no**
> - Continue to configure the system with these values? [no]: **yes** (_approx. 15-20 mins_)

---
add services on firewall to use the FreeIPA server functionality
- `firewall-cmd --permanaent --add-service={ntp,http,https,ldap,ldaps,kerberos,dns} --permanent` - to allow inbound traffic
- `firewall-cmd --reload` - to reload the firewall

