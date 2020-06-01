# [RHCSA 8 Practice Exam](https://rhcsapracticequestions.com/questions/questions)

This practice exam is for those that have gone through an RHCSA course/book and want to test their knowledge before sitting the RHCSA 8 exam .


# 

1.) Add 3 users: harry, natasha, tom. The requirements: The Additional group of the two users: harry, natasha is the admin group. The user: tom's login shell should be non-interactive.

- `useradd -G admin harry`
- `useradd -G admin natasha`
- `useradd -s /sbin/nologin tom`
- `id harry;id natasha` - to show addt'l group
- `cat /etc/passwd` - to show the login shell

# 

2.) Plan to run echo hello command at 14:23 every day.

- `crontab -e`
- `23 14 * * * /bin/echo hello`
- `crontab -l` - to verify

# 

3.) Find the files owned by harry, and copy it to catalog: /opt/dir

- `cd /opt/`
- `mkdir dir`
- `find / -user harry -exec cp -rfp {} /opt/dir/ \;` - [(`-r` - recursive `-f` - force `-p` - preserve)](http://linuxcommand.org/lc3_man_pages/cp1.html)

# 

4.) Create a 2G swap partition which takes effect automatically at boot-start, and it should not affect the original swap partition.

- `fdisk /dev/sdb`
- `n` - `"Enter"` - `+2G` - `t` `82` - `:wq`
- `partprobe`
- `mkswap /dev/sdb6`
- `vi /etc/fstab` - no mount point
> `/dev/sdb6` `swap` `swap` `defaults  0 0` (make entry in `fstab` file)
- `swapon -a` - to enable / activate SWAP as per entry in `fstab` file (**no mount needed**)
- `free -m` - to verify added SWAP

# 

5.) Create a user named alex, and the user id should be 1234, and the password should be alex111.

- `useradd -u 1234 alex`
- `passwd alex` - `alex111`

# 

6.) Securely copy /root/shared files from your machine to “server2.example.com” to /shared directory, ssh connection is not established, use the password "password"

- `scp /root/shared server2.example.com:/shared/` & enter password

# 

7.) Find all the files owned by user “alex” and redirect the output to /home/alex/files

- `find / -user alex -type f > /home/alex/files`

# 

8.) Configure the IP 192.168.122.10 on eth0 interface on ‘system.eight.example.com’ and set the DNS IP as 192.168.122.254. Configure the Default Gateway as 192.168.122.1. The IP assigned must be static.

- `systemctl status NetworkManager`
- `nmcli connection show`
- `ip a`
- `nmcli connection add con-name system type ethernet ifname eth0 ipv4.address 192.168.122.10/24 ipv4.gateway 192.168.122.1 ipv4.dns 192.168.122.254 ipv4.method manual` (**is "system" the name of the server, or is it always "system"???**) 
- `nmcli connection up system`
- `systemctl restart NetworkManager`
- `cd /etc/sysconfig/network-scripts` - verify conenction settings
- `cat /etc/resolv.conf` - verify DNS IP address
- route -n` - verify default gateway


# 

9.) Copy all the lines starting with "ted" "Ted" from ted.txt and copy it to file onlyted.txt

- `cat ted.txt | grep -i ^ted > /onlyted.txt`
- `cat onlyted.txt` - verify

# 

10.) Schedule a script /myawesomescript.sh as user “nick” which should be executed 12:15 every Tuesday.

Make sure that only nick can schedule cron jobs as a non-root user

- `vi /etc/cron.allow` & add `nick` to file
- `su - nick` - switch to nick
- `crontab -e`
- `15 12 * * 2 /myawesomescript.sh`
- `crontab -l` - to view job

# 

11.) Schedule a script /myawesomescript.sh as user “ sam ” which should be executed every 15 minutes

- `crontab -u sam -e` - as root
- `*/15 * * * * /myawesomescript.sh`
- `crontab -u sam -l`

# 

12.) Change the default password policy so that newly created users have passwords that must be changed every 60 days

- `vi /etc/login.defs` & change `PASS_MAX_DAYS 60` 

# 

13.) Add group called testers with GID 30000

- `groupadd -gid 30000 testers`

# 

14.) Create users tester1, tester2, tester3 with testers as their supplementary group

- `useradd -G testers tester1`
- `useradd -G testers tester2`
- `useradd -G testers tester3`

# 

15.) Force all the testers to change their password whenever they log on to the system for the first time

- `chage -d 0 tester1`
- `chage -d 0 tester2`
- `chage -d 0 tester3`

# 

16.) Configure administrative rights for all the members of the testers group (**REVIEW**)

- `vi /etc/sudoers.d/testers` - new file called `/etc/sudoers.d/testers`
- `%testers ALL=(ALL) ALL` - add the entry
- `cat /etc/sudoers.d/testers` - view file contents

# 

17.) Set the users account for tester1, tester2, tester3 to expire in 30 days (**REVIEW**)

- `date -d "+30 days" +%F` - use `date` to get the time after 30 days

    - use the time to set the account expiry:
- `chage -E 2020-06-03 tester1` 
- `chage -E 2020-06-03 tester2`
- `chage -E 2020-06-03 tester3`

# 

18.) Change the password policy for tester3 to require a new password every 10 days

- `chage -M 10 tester3` 

Important to know these [differences](https://www.cyberciti.biz/faq/understanding-etcshadow-file/), some questions ask about the **account expiring**, and some of the questions ask about **requiring a new password**

Also, this command is what you use for an individual user, if the question asks about the default maximum age for all the users, the you need to change the file: `/etc/login.defs`

# 

19.) Create a volume group called "shazam" consisting of two physical volumes each 256MB in size

- `fdisk /dev/sdb`
>`n` - `e` - "Enter" - "Enter" - `n` - "Enter" - `+256M` - "Enter" - `t` - "Enter" - `8e` (change partition to "LVM")

- do the same thing again to create the second partition.

- `partprobe`

- `pvcreate /dev/sdb8 /dev/sdb9`

- `vgcreate shazam /dev/sdb8 /dev/sdb9`

- `vgdisplay`

# 

20.) Add a logical volume called storage in the volume group shazam mounted at /storage permanently as xfs

- `lvcreate -n storage -L 400M shazam`
- `lvdisplay`
- `mkfs -t xfs /dev/shazam/storage`
- `mkdir /storage`
- `vi /etc/fstab`
> `/dev/shazam/storage  /storage  xfs  defaults  1  2`
- `mount -a`

# 

21.) Extend the logical volume storage to be 700MB

- `fdisk /dev/sdb`
- `n` - "Enter" - "Enter" - `+512M` - `t` - "Enter" - `8e` - `wq`
- `partprobe`
- `lsblk` - check new partitions
- `vgextend shazam /dev/sdb10`
- `vgdisplay`
- `lvextend -r -L 700M /dev/shazam/storage`
- `df -h` - verify changes

# 

22.) 

- `

# 

23.) 

- `

# 

24.) 

- `

# 

25.) 

- `

# 

26.) 

- `

# 

27.) 

- `

# 

28.) 

- `

# 

29.) 

- `

# 

30.) 

- `

# 

31.) 

- `

# 

32.) 

- `

# 

33.) 

- `

# 

34.) 

- `

# 

35.) 

- `

# 

36.) 

- `

# 

37.) 

- `

# 

38.) 

- `

# 

39.) 

- `

# 

40.) 

- `

# 

41.) 

- `

# 

42.) 

- `

# 

43.) 

- `

# 

44.) 

- `

# 

45.) 

- `

# 

46.) 

- `

# 

47.) 

- `

# 