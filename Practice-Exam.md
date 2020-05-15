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

1.) Interrupt the boot process and reset the root password. Change it to “wander” to gain access to the system.

2.) Repos are available from the repo server at http://repo.eight.example.com/BaseOS and and http://repo.eight.example.com/AppStream for you to use during the exam.

3.) The system time should be set to your (or nearest to you) timezone and ensure NTP sync is configured.

4.) Add the following secondary IP addresses statically to your current running interface. Do this in a way that doesn’t compromise your existing settings:
>IPV4 - 10.0.0.5/24\
IPV6 - fd01::100/64

5.) Enable packet forwarding on system1. This should persist after reboot.

6.) System1 should boot into the multiuser target by default and boot messages should be present (not silenced).

7.) Create a new 2GB volume group named “vgprac”.

8.) Create a 500MB logical volume named “lvprac” inside the “vgprac” volume group.

9.) The “lvprac” logical volume should be formatted with the xfs filesystem and mount persistently on the /mnt/lvprac directory.

10.) Extend the xfs filesystem on “lvprac” by 500MB.

11.) Use the appropriate utility to create a 5TiB thin provisioned volume.

12.) Configure a basic web server that displays “Welcome to the web server” once connected to it. Ensure the firewall allows the http/https services.

13.) Find all files that are larger than 5MB in the /etc directory and copy them to /find/largefiles

14.) Write a script named awesome.sh in the root directory on system1.
> If “me” is given as an argument, then the script should output “Yes, I’m awesome.”\

> If “them” is given as an argument, then the script should output “Okay, they are awesome.”\

> If the argument is empty or anything else is given, the script should output “Usage ./awesome.sh me|them”

15.) Create users phil, laura, stewart, and kevin.
> All new users should have a file named “Welcome” in their home folder after account creation.

> All user passwords should expire after 60 days and be atleast 8 characters in length.

> phil and laura should be part of the “accounting” group. If the group doesn’t already exist, create it.

> stewart and kevin should be part of the “marketing” group. If the group doesn’t already exist, create it.

16.) Only members of the accounting group should have access to the “/accounting” directory. Make laura the owner of this directory. Make the accounting group the group owner of the “/accounting” directory.

17.) Only members of the marketing group should have access to the “/marketing” directory. Make stewart the owner of this directory. Make the marketing group the group owner of the “/marketing” directory.

18.) New files should be owned by the group owner and only the file creator should have the permissions to delete their own files.

19.) Create a cron job that writes “This practice exam was easy and I’m ready to ace my RHCSA” to /var/log/messages at 12pm only on weekdays.