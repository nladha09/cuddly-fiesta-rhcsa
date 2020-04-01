
## Lab Chapter will include: 
install & configure LDAP and integrated DNS server w/ 3 IPA server solution

In the exam you will already have this machine pre-configured:

![](https://res.cloudinary.com/dlsvazigf/image/upload/v1585774617/lab-environment.jpg)

__________

## Differences b/w RHEL 8 & RHEL 7 from RHCSA exam point of view:
1.) **Repository** - RHEL 8 is distributed through (2) main repos - make sure you define two repos on RHEL 8:
- Base OS
- AppStream

The concept of *modules* is introduced - modules are collections of packages representing a logical unit: an app, language stack, DB, or a set of tools.

2.) **Changes in YUM stack** - on RHEL 8, installing software is ensured by the new version of the YUM tool, which is based on DNF technology (YUM v4).

Advantages of YUM v4 over YUM v3:
- increased performance
- support for modular content

3.) **Time Synchronization** - in RHEL 8, the HTP protocol is implemented only by the *chronyd* daemon, provided by the *chrony* package.

- Conversely, RHEL 7 supported (2) implementations of the NTP protocol: *ntp* & *chrony*
- The *ntp* daemon is no longer available - if you used ntp on RHEL 7, you might need to migrate to *chrony*

4.) **Networking** - default tool for network management is *NetworkManager*. 

- In RHEL 8, to run the `ifup` & `ifdown` scripts, *NetworkManager* must be running.
- The basic installation provides a new version of the `ifup` & `ifdown` scripts which call *NetworkManager* theough the `nmcli` tool.
- *Network scripts* are deprecated in RHEL 8 & are no longer provided by default.








