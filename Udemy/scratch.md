any tips and notes go here:

- `man -K "thing you're searching for"` will help if stuck
---

- `yum provides <add_help_text_here>` - to search for what you need to install for a particular task

---

> <span style="font-family:courier new">**Configure Apache to server a basic website that shows the test "Hello World" after connecting to it**:</span>

Commands:
- `yum install httpd` - need to install httpd as apached is the `httpd` package
- `systemctl status httpd` - check status
- `systemctl start httpd` - start service (although may be already running)
- `systemctl enable httpd` - to enable httpd
- `cd /var/www/html` - go into the `httpd` main directory & create an `index.html` file (`touch index.html`)
- `vi index.html` - add the text "Hello World" in this file & then `:wq!`
- `systemctl restart httpd` - after making this change we need to restart the `httpd` service - (changes & website should now be active)
- `curl http://localhost` - test changes w/ CURL (pending no service issues or blocks by firewalls / SELinux, we should see "Hello World" returned to us.

---

> <span style="font-family:courier new">**Find all files in `/etc` that are smaller than 1000bytes & copy those to `/tmp/files/pictures`**:</span>

Commands:
- `find /etc -type f -size -1000c -exec cp -pv {} pictures \;` - this will find all files in `/etc` w/ a size less than (`-`) 1000c & then `-exec`(ute) the `cp` command on the resulting files `{}` & copy them to the relative path w/in our directory of 'pictures', & then close the command using `/;` ((`-pv` for verbose & save permissions))
- `ls pictures/` - now we should see everything we intended to copy being now copied there.
---

**TODO**:
- Preserve system journals is a new objective, system journals not being preserved by default.
     - `vi /etc/systemd/journald.conf` - change "_Storage_" value to "_persistent_" from "_auto_"
     - `systemctl restart systemd-journald` - restart systemd-journal once changes are saved
     - `var/log/journal` - directory should be created
     
- ~~Mount and unmount CIFS network file systems objective has been withdrawn~~ but not NFS network file systems: SAMBA and Windows integration doesn’t seem to be a priority anymore, at least on the filesystem side.
- Mount and unmount network file systems using NFS (**autofs**)
- Configure disk compression objective shows up: Red Hat wants sysadmins to master the **VDO** technology, purchased several years ago.
- Manage layered storage is a new objective dealing with **Stratis**, the Red Hat‘s answer to the ZFS file system: not a new product but a better integration between LVM and xfs.
- Configure time service clients objective replaces the previous Configure a system to use time services objective: Red Hat doesn’t ask candidates to set up time service client and server anymore but only time service clients. Knowledge of the NTP daemon is no longer necessary, that of **Chrony** is enough.
- Configure **IPv4 and IPv6** addresses objective, previously associated with the RHCE 7 exam, is a new RHCSA 8 objective.
- Restrict network access using **firewall-cmd/firewalld** and Configure firewall settings using firewall-cmd/firewalld are not really new objectives.
- Configure **superuser access** is a new objective, which asks candidates to know the sudo command and associated configuration.
- **SELinux** context stuffs

---

Questions -

- ` do you have to use `more` when using `grep` (?)
- `tar` don't have to specifiy `j` or `z` when compressing (?)
- sticky bits & `SGID` - getting more comfortable
     - do you have to have "read" perms in order to "execute"? 