# Understand & Use Essential Tools

## Making an SSH Connection

---

> <span style="font-family:courier new">**Task 1. Establish SSH connection to "server2" from "server1"**:
>> - User user "riya" to make this connection.</span>

Commands:
- `systemctl status sshd` - to check the status of SSH service
- `adduser riya` - add user riya if not already added - (on the **remote** machine - user doesn't need to exist on local machine)
- `passwd riya` - to set the password for riya so user can log in (again, this needs to be done at least only on the **remote** machine)
- `ssh riya@ipaserver.example.com` - to establish ssh connection
- Enter password for riya: ****** (ex: password)
- `hostname` - to verify "riya" is connected to "server2"

## Securely copying files using scp

---

> <span style="font-family:courier new">**Task 2. Securely copy `/root/details` file from "server1" to "server2" to `/backup` directory.**:
>> - Use the password "password" for this task.</span>

Commands:
- `man scp` - manual page for scp
- `scp /root/details server2:/backup/` - to transfer files securely w/ scp
- Enter password for root: ****** (ex: password)

![name](images/name.jpg)


## Use of `grep` to match lines starting w/ pattern

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![name](images/name.jpg)


## Use of `grep` for non-matching pattern (invert-match)

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![name](images/name.jpg)


## Use `find` command to list files owned by user

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![name](images/name.jpg)


## Using `find` command to locate file using name of file

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![name](images/name.jpg)


## Finding files with extension .txt using `find` command

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![name](images/name.jpg)


## Use of `find` command to save all directories owned by user

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![name](images/name.jpg)


## Use of find command to list all directories & files based on UID

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![name](images/name.jpg)


## Use of `tar` command to archive and compress contents of directory

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![name](images/name.jpg)


## Use of `tar` command to extract the data from archive

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![name](images/name.jpg)


## Use of `tar` to archive contents with `gzip` compression

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![name](images/name.jpg)


## Decompressing files using gunzip & bunzip2

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![name](images/name.jpg)


## Setting `ugo-rwx` permissions on filesystems

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![name](images/name.jpg)


## Creation of symbolic links

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![name](images/name.jpg)
