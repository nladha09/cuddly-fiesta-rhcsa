# Manage Basic Networking

## Configure IPv6 address & DNS

---

> <span style="font-family:courier new">**Task 1. Configure eth0 interface with IPv6 address 2020::1/64 & set DNS address as 2020::2**:
>> - Already existing IPv4 network configurations should not be impacted</span>

Commands:
- `nmcli connection modify <connection_name_system> ipv6.addresses 2020::1/64 ipv6.dns 2020::2 ipv6.method manual` - to configure IPv6 on eth0 interface (do not use `nmcli connection add` - in this case it will override the existing connection) (here `64` is a network identifier; we know ipv6 address is 128-bits long; first 48-bits = global routing prefix, next 64-bits = subnet ID; next 64-bits = network identifier)
- `nmcli connection up <connection_name_system>` - to reset the connection
- `ip addr` - to display network configurations
- `nmcli connection show <connection_name_system>` - to display connection information
- `cat /etc/resolv.conf` - to verify DNS IP

## Configuring Static Route

---

> <span style="font-family:courier new">**Task 2. Configure static route on "server1" for destination 10.1.1.0/24 via 192.168.122.254**:
>> - Route configuration must be persistent after reboot.
>> - eth0 should be used as exit interface.</span>

Commands:
- `ip route add 10.1.1.0/24 via 192.168.122.254` - to add static route in runtime
- `ip route show or route -n` - to display the static route

---

Add persistent route with command line:
- `nmcli connection modify system ipv4.routes "10.1.1.0/24 192.168.122.254"`

---

Add persistent route with configuration file:
- `vi /etc/sysconfig/network-scripts/route-system`
- `10.1.1.0/24 via 192.168.122.254`
- `:wq`
- `nmcli connection up <connection_name_system>` - to reset the connection
- `ip route sh` - to display

## Restrict Network access for Specific service using firewall-cmd/firewall

---

> <span style="font-family:courier new">**Task 4. Configure "server1" machine to restrict ssh access to 192.168.122.0/24 network**:</span>

Commands:
- `firewall-cmd --list-all` - to list all firewall configurations
- `man firewalld.richlanguage` - man page for firewall rich language (Example 3)
- `firewall-cmd --add-rich-rule='rule family="ipv4" source address="192.168.122.0/24" service name ="ssh" accept' --permanent` - to add rich rule to accept ssh traffic only from 192.168.122.0/24 network
- remove ssh service from services list, if you don't remove ssh service, then rich rule configured to accept ssh traffic from 192.168.122.0/24 network only will not be effective. This is due to order in which `firewalld` evaluates the different definitions on firewall. If `firewalld` will find ssh service on services list, it will allow access irrespective of accessing network & rich rule will be ignored.
- `firewall-cmd --remove-service=ssh --permanent` - persistent change
- `firewall-cmd --reload` - to reload the firewall to make the change done in above command to be effective
- `firewall-cmd --list-all` - to list all firewall configurations & check if newly created rule is now available

---


to test this:
- we have only one network so it is not possible to test this, to test this working of rule, you just add this rule to allow access for some host not on 192.168.122.0/24 network & then test ssh connection from "server2", it must be denied.

**TODO**:
- [ ]  test
