# Lab Set Up (not on exam)

## Installing LDAP & Integrated DNS Server w/ FreeIPA Server Solution (RHEL 8)

---

Commands:
- `yum install -y @idm:DL1` - to install identity management system module [`@idm` = module; `DL1` = stream]
- `yum install -y freeipa-server bind-dyndb-ldap ipa-server-dns` - to install all required packages
- `ipa-server-install` - to install FreeIPA server
> - You might run into this error when running the `ipa-server-install` cmd (if machine is connected to the internet):
```
DNS zone example.com already exists in DNS and is handled by servers: a.iana-server.net., b.iana-servers.net...
```
> - if so, use `ipa-server-install --allow-zone-overlap` instead of `ipa-server-install`

> - Do you want to configure integrated DNS (BIND)? [no]: **yes**
> - Server host name [ipaserver.example.com]: **ipaserver.example.com**
> - Please confirm the domain name [example.com]: **example.com**
> - Please provide a realm name [EXAMPLE.COM]: **EXAMPLE.COM**
> - Directory Manager password: ********* (**password**)
> - Password (confirm): *********
> - IPA admin password: ********* (**adminadmin**)
> - Password (confirm): *********
> - Please provide the IP address to be used for this host name: **192.168.122.254**
> - Do you want to configure DNS forwarders? [yes]: **no**
> - Do you want to search for missing the reverse zone? [yes]: **no**
> - Continue to configure the system with these values? [no]: **yes** (_approx. 15-20 mins_)

---
add services on firewall to use the FreeIPA server functionality
- `firewall-cmd --permanent --add-service={ntp,http,https,ldap,ldaps,kerberos,dns} --permanent` - to allow inbound traffic
- `firewall-cmd --reload` - to reload the firewall

## Creating Forward & Reverse DNS Zones & adding DNS entries

---

managing DNS zones & DNS record entries:

- `kinit admin` - to authenticate as IPA admin
- `ipa dnszone-add 122.168.192.in-addr.arpa` - to add reverse zone by name
- `ipa dnsrecord-add 122.168.192.in-addr.arpa. 254 --ptr-rec ipaserver.example.com` - adding PTR record for 192.168.122.254
- `ipa dnsrecord-add example.com system --a-rec 192.168.122.10` - adding A records for system.example.com
- `ipa dnszone-show example.com` - to query the DNS zone
- `host system.example.com` - to query DNS for hostname (forward DNS lookup)
- `host 192.168.122.10` - to query DNS for IP (reverse DNS lookup)
- > you can also use `dig` or `nslookup` utility for querying DNS server





**TODO**:
- [x] ~~understand differences b/w `yum` & `rpm`~~
- [x] ~~there is one more prompt that is not covered - "Do you want to configure chronyd with NTP server or pool address? [no]:" if I respond with "yes" I am asked "Enter NTP source server addresses seperated by comma, or press Enter to skip:" & then "Enter a NTP source pool address, or press Enter to skip:" --- when attempting to synchronize time it couldn't find NTP servers so it uses default chrony config.~~

- [ ] figure out CA cert error when installing Free IPA server (image on cloudinary)

