# Deploy, Configure, & Maintain Systems

## Configure system to boot in Graphical Target

---

> <span style="font-family:courier new">**Task 1. Execute command to change system to run in "Graphical" target. Make this setting persistent & system should boot in same target on next reboot**:</span>

![linux-config-pic-run-levels](images/linux-config-pic-run-levels.jpg)

Commands:
- `systemctl get-default` - to display the current SYSTEMD (Linux initialization system & service manager; provides logging daemon & other tools & utils for common admin tasks)
- `systemctl isolate graphical.target` - to change the SYSTEMD target in current session
- `systemctl set-default graphical.target` - to set the SYSTEMD target which system will use by default
- `cd /usr/lib/systemd/system` - directory containing SYSTEMD units of installed packages
- `cd /etc/systemd/system` - directory containing local SYSTEMD configurations
- `man runlevel` - *runlevels man page*
- `man systemd.unit` - manual page for SYSTEMD units

## Assign Host-Name to Machine

---

Commands:

## Configuring static IPv4 address on Interface (RHEL 8)

---

Commands:

## Yum repositories BaseOS & AppStream in (RHEL 8)

---

Commands:

## Create local Yum repositiory & Yum groups from ISO-file (RHEL 8)

---

Commands:

## Configure System to use BaseOS & AppStream Repositories (RHEL 8)

---

Commands:

## Scheduling job using crontab for other user as root user

---

Commands:

## Scheduling cron jobs as user other than root

---

Commands:

## Scheduling jobs using at command

---

Commands:

## Configure Service to start System boot

---

Commands:

## Working with package Module streams (RHEL 8)

---

Commands:

## Modifying Bootloader (GRUB2) settings

---

Commands:


**TODO**:
- [ ] for practice-env question: Thank you for creating this practice env; it saves a lot of time and learned a bit about Ansible & Vagrant in implementing the set up. Will `systemctl isolate graphical.target` or `systemctl set-default graphical.target` not work on the environments b/c the initial creation was not "server w/ GUI"? 
