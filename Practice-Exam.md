# RHCSA 8 Practice Exam
## Introduction

This practice exam is for those that have gone through an RHCSA course/book and want to test their knowledge before sitting the RHCSA 8 exam .

**Optional Automatic Exam Setup Available**
> Here is an automated exam environment deployment for Mac/Linux/Windows that deploys the practice exam environment for you, including IPA server/client installation and configuration. You can also use your own lab environment. Navigate to the below repo and follow the README instructions: https://github.com/rdbreak/rhcsa8env

It’s recommended you make a separate ~/bin directory and then clone the environment. Check the included README.md for the environment info and instructions. Reach out to @rdbreak with any questions or concerns.

**Systems Set Up**
- Hostname - system1.eight.example.com
>IP - 192.168.55.150/24\
DNS - 8.8.8.8\
GW - 192.168.5.1
- Hostname - system2.eight.example.com
>IP - 192.168.55.151/24\
DNS - 8.8.8.8\
GW - 192.168.5.1

If you’re not using the automated deployments, then install RHEL 8 with  “Workstation”, then add “Development Tools, Graphical Administration Tools, and Container tools” to your installation. Configure the partitions as follows:

1.) 15GB disk space with LVM partitions.
>10GB - /\
1GB - swap\
1GB - boot\
4GB - unallocated space

2.) If you’re using a custom environment, then add an additional 5GB disk for use in the exam. If you’re using the automated deployment, then an additional disk is already supplied for you to use.

**NOTE** - The below questions assume you’re using the automated deployment but you can also use a practice environment you created. However, you will have to set up your own repo, change host names, IP addresses, etc to reflect your own environment details.

# 

1.) Interrupt the boot process and reset the root password. Change it to “wander” to gain access to the system.

- `rd.break` - at the end of the line starting with "Linux" & then "CTRL+X" to continue process
- `mount -o remount, rw /sysroot`
- `chroot /sysroot`
- `passwd` - enter password to set
- `touch /.autorelabel`
- `exit` & `exit`

# 

2.) Repos are available from the repo server at http://repo.eight.example.com/BaseOS and and http://repo.eight.example.com/AppStream for you to use during the exam.

# 

3.) The system time should be set to your (or nearest to you) timezone and ensure NTP sync is configured.

- `timedatectl set-timezone America/Denver`
- ` vi /etc/chrony.conf`
- `chronyc sources`

# 

4.) Add the following secondary IP addresses statically to your current running interface. Do this in a way that doesn’t compromise your existing settings:
>IPV4 - 10.0.0.5/24\
IPV6 - fd01::100/64

- `systemctl status NetworkManager`
- `systemctl start NetworkManager`
- `systemctl enable NetworkManager`
- `nmcli connection modify server1 ipv4.addresses 10.0.0.5/24 ipv6.addresses fd01::100/64 ipv4.method manual ipv6.method manual`
- `nmcli connection up "System eth0"`
- `systemctl restart NetworkManager`
- `nmcli connection show`
- `ip a`

# 

5.) Enable packet forwarding on system1. This should persist after reboot.

- `sysctl -a | grep forwarding >> /etc/sysctl.conf`
- `vi /etc/sysctl.conf`
- **NEED TO FIND OUT WHICH KERNEL PARAMETER IT IS ASKING FOR** (will the parameter be specified
- `net.ipv4.ip_forward = 1` - change `0` to `1` 

# 

6.) System1 should boot into the multiuser target by default and boot messages should be present (not silenced).

- `systemctl set-default multi-user.target` (this should already be in effect)

- `vi /etc/default/grub`

# 

7.) Create a new 2GB volume group named “vgprac”.

- `fdisk /dev/sdb` - to create new partition
    - `n` & then press "ENTER"
    - `+2G`
    - `t` & press "ENTER"
    - `8e` & `wq`
- `partprobe` - to inform kernel about partition
- `pvcreate /dev/sdb8` - create physical volume on partition
- `vgcreate vgprac /dev/sdb8` - create volume group (do I need to specify "+2G" here?)

# 

8.) Create a 500MB logical volume named “lvprac” inside the “vgprac” volume group.

- `lvcreate -n lvprac -L +500M /dev/sdb8 lvprac` - (check if this is correct?)
- `lvdisplay | less` - to view LV info

# 

9.) The “lvprac” logical volume should be formatted with the xfs filesystem and mount persistently on the /mnt/lvprac directory.

- `mkdir /mnt/lvprac`
- `mkfs -t xfs -f /dev/vgprac/lvprac`
- `vi /etc/fstab` & `/dev/vgprac/lvprac  /mnt/lvprac  xfs  defaults  0 0`
- `mount -a`

# 

10.) Extend the xfs filesystem on “lvprac” by 500MB.

- `lvextend -r -L +500M /dev/vgprac/lvprac`
- `lvdisplay`

# 

11.) Use the appropriate utility to create a 5TiB thin provisioned volume.

- **LESSON 15 in OneNote covers this (at the bottom)**

- VDO currently supports any logical size up to 254 times the size of the physical volume with an absolute maximum logical size of 4PB.

- `yum install vdo kmod-kvdo` - to install VDO (two packages)
- `systemctl status vdo.service` - check status, but should be enabled & do not need to activate (will start automatically)
- `fdisk /dev/sdb`
    - `n` press "Enter" twice & `:wq` - 5 TiB = 5120 GiB; so I created a physical partition of +15G (which worked - this is the physical size)
    - `partprobe` - to inform kernel of partition

- `vdo create --name=vdo1 --device=/dev/sdb1 --vdoLogicalSize=5T` - this will create the vdo (logical size)
- `vdo list` - list VDO devices
- `vdo -h` - helpful to go through these

(**need to mount as well**?)

- [Recommended VDO Slab Sizes by Physical Volume Size](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/storage_administration_guide/vdo-integration#vdo-ig-volume)

# 

12.) Configure a basic web server that displays “Welcome to the web server” once connected to it. Ensure the firewall allows the http/https services.

- `yum install httpd` - install apached - which is the `httpd` package
- `systemctl status httpd` 
- `systemctl start httpd`
- `systemctl enable httpd` - enable it
- `cd /var/www/html` - go into the `httpd` main directory & create `index.html` file
- `touch index.html` & `vi index.html` (add the text "Welcome to the web server" in this file & `:wq`)
- `systemctl restart httpd` - after making this change, we need to restart the `httpd` service (changes & website should now be active)
- `curl http://localhost` - test with CURL - pending no service issues or blocks by firewalld / SELinux, we should see 'Welcome ot the seb server' returned to us

# 

13.) Find all files that are larger than 5MB in the /etc directory and copy them to /find/largefiles

- `find /etc -type f -size -1000c -exec cp -pv {} /find/largefiles /;`

# 

14.) Write a script named awesome.sh in the root directory on system1.

- **LESSON 19 in OneNote covers this**

- `touch awesome.sh`
- `vi /awesome.sh`

> If “me” is given as an argument, then the script should output “Yes, I’m awesome.”

> If “them” is given as an argument, then the script should output “Okay, they are awesome.”

> If the argument is empty or anything else is given, the script should output “Usage ./awesome.sh me|them”

``` bash
# !/bin/bash

# check that an argument was provided or exit
if [ -z $1 ]
then
    echo "Usage ./awesome.sh me|them"
    exit 2
fi

# evaluate arguments
case $1 in
me)
    echo "Yes, I'm awesome"
    ;;
them)
    echo "Okay, they are awesome"
    ;;
*)
    echo "Usage ./awesome.sh me|them"
esac
```
- `chmod +x awesome.sh` - set permissions on this file to execute

to test: 

- `./awesome.sh`
- `./awesome.sh me`
- `./awesome.sh them`
- `./awesome.sh blahblah`

# 

15.) Create users phil, laura, stewart, and kevin.

> All new users should have a file named “Welcome” in their home folder after account creation.

- `touch /etc/skel/welcome.txt`
- `useradd -m phil`
- `useradd -m laura`
- `useradd -m stewart`
- `useradd -m kevin`

[From](https://unix.stackexchange.com/questions/146896/default-files-in-home-dir-for-each-user) `man useradd` :

```
-k, --skel SKEL_DIR
           The skeleton directory, which contains files and directories to 
           be copied in the user's home directory, when the home
           directory is created by useradd.

           This option is only valid if the -m (or --create-home) option is 
           specified.

           If this option is not set, the skeleton directory is defined by 
           the SKEL variable in /etc/default/useradd or, by
           default, /etc/skel.

           If possible, the ACLs and extended attributes are copied.
```

> All user passwords should expire after 60 days and be atleast 8 characters in length.

- `vi /etc/login.defs`

```
# cat /etc/login.defs
#
# Please note that the parameters in this configuration file control the
# behavior of the tools from the shadow-utils component. None of these
# tools uses the PAM mechanism, and the utilities that use PAM (such as the
# passwd command) should therefore be configured elsewhere. Refer to
# /etc/pam.d/system-auth for more information.
#

# *REQUIRED*
#   Directory where mailboxes reside, _or_ name of file, relative to the
#   home directory.  If you _do_ define both, MAIL_DIR takes precedence.
#   QMAIL_DIR is for Qmail
#
#QMAIL_DIR	Maildir
MAIL_DIR	/var/spool/mail
#MAIL_FILE	.mail

# Password aging controls:
#
#	PASS_MAX_DAYS	Maximum number of days a password may be used.
#	PASS_MIN_DAYS	Minimum number of days allowed between password changes.
#	PASS_MIN_LEN	Minimum acceptable password length.
#	PASS_WARN_AGE	Number of days warning given before a password expires.
#
PASS_MAX_DAYS	99999
PASS_MIN_DAYS	0
PASS_MIN_LEN	5
PASS_WARN_AGE	7

#
# Min/max values for automatic uid selection in useradd
#
UID_MIN                  1000
UID_MAX                 60000
# System accounts
SYS_UID_MIN               201
SYS_UID_MAX               999

#
# Min/max values for automatic gid selection in groupadd
#
GID_MIN                  1000
GID_MAX                 60000
# System accounts
SYS_GID_MIN               201
SYS_GID_MAX               999

#
# If defined, this command is run when removing a user.
# It should remove any at/cron/print jobs etc. owned by
# the user to be removed (passed as the first argument).
#
#USERDEL_CMD	/usr/sbin/userdel_local

#
# If useradd should create home directories for users by default
# On RH systems, we do. This option is overridden with the -m flag on
# useradd command line.
#
CREATE_HOME	yes

# The permission mask is initialized to this value. If not specified, 
# the permission mask will be initialized to 022.
UMASK           077

# This enables userdel to remove user groups if no members exist.
#
USERGROUPS_ENAB yes

# Use SHA512 to encrypt password.
ENCRYPT_METHOD SHA512
```

> phil and laura should be part of the “accounting” group. If the group doesn’t already exist, create it.

- `groupadd accounting`
- `usermod -aG accounting phil`
- `usermod -aG accounting laura`

> stewart and kevin should be part of the “marketing” group. If the group doesn’t already exist, create it.

- `groupadd marketing`
- `usermod -aG marketing stewart`
- `usermod -aG marketing kevin`

16.) Only members of the accounting group should have access to the “/accounting” directory. Make laura the owner of this directory. Make the accounting group the group owner of the “/accounting” directory.

- `chown laura:accounting /accounting` ( < **CHECK ON THIS**)  

# 

17.) Only members of the marketing group should have access to the “/marketing” directory. Make stewart the owner of this directory. Make the marketing group the group owner of the “/marketing” directory.

- `chown stewart:marketing /marketing` ( < **CHECK ON THIS**)  

# 

18.) New files should be owned by the group owner and only the file creator should have the permissions to delete their own files.

[sticky-bits & SGID](http://www.techcuriosity.com/resources/linux/advanced_file_permissions_in_linux.php)

# 

19.) Create a cron job that writes “This practice exam was easy and I’m ready to ace my RHCSA” to /var/log/messages at 12pm only on weekdays.

- `crontab -e`
- `* 12 * * 3 echo "I'm ready to ace my RHCSA" > /var/log/messages` ( < **CHECK ON THIS**)  
