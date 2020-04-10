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

Setup: 
- `touch /root/details` - create the file on "*server1*".
- `vi /root/details` - add something to the file.
- `cat /root/details` - view the details of the file.
- `cd / && mkdir backup` - on "*server2*" create backup directory under root file system.
- `ls -l | grep backup` - verify backup directory has been created.

Commands:
- `man scp` - manual page for scp
- `scp /root/details server2:/backup/` - on "*server1*" source path and destination path - to transfer files securely w/ scp (**the trailing `/` is important to signify directory**)
- Enter password for root: ****** (ex: password)
- `cd /backup/ && ls` - shows "details" file has been copied; can also cat the file to view contents `cat details`.

- **NOTE** - this task uses root to transfer the file, however, it is not recommended in the real world. To transfer as a user you would use the following command: `scp /root/details riya@server2:/backup/` however, the /backup/ directory is owned by root user, and the user riya does not have the right permissions.


## Use of `grep` to match lines starting w/ pattern

---

> <span style="font-family:courier new">**Task 3. Copy all lines starting with the word "Sed" or "sed" from /file.txt * copy to file "/word/file"**:</span>

Commands:
- `man grep` - manual page for grep
- `more /file.txt | grep -i ^sed > /word/file` - pipe `/file.txt` to `grep`; `-i` = ignore-case; `^` used to search a line with a particular pattern (ex: "sed") and redirect the output to `/word/file`.
- `more /word/file` - verify task with `more` command and file name.

- **NOTE** - `more /file.txt | grep -i sed | wc` will give you the lines (`-l`), words (`-w`), and char (`-c`) count.


## Use of `grep` for non-matching pattern (invert-match)

---

> <span style="font-family:courier new">**Task 4. Copy all line not containing "sEd" or "SeD" from file /root/rhcsa.txt to /root/results.txt file**:</span>

Commands:
- `more /rhcsa.txt | grep -v -i "sEd" > /root/results.txt` - to copy line not containing "sEd" or "SeD".
- `more /root/results.txt` - to verify the results


## Use `find` command to list files owned by user

---

> <span style="font-family:courier new">**Task 5. Find all the files owned by user "riya" & redirect the output to /root/riya_files**:</span>

Commands:
- `find / -user riya -type f > /root/riya_files` - search in `/` filesystem; next is search criteria; `-type` option to list "files" (not directories, which would be `-type d` ) to copy files owned by riya

- **NOTE** - you might see some standard errors `No such file or directory` which may mean the file does not exist anymore. You can ignore this.


## Using `find` command to locate file using name of file

---

> <span style="font-family:courier new">**Task 6. Locate the file "dummy.txt" searching through "/" file system & save the output in /location file.**:</span>

Commands:
- `find / -name "dummy.txt" -type f > /location` - to find file with the name "dummy.txt."
- `cat /location` - to verify the results


## Finding files with extension .txt using `find` command

---

> <span style="font-family:courier new">**Task 7. Locate the files with extension ".txt" searching through "/" file system & save the putput in "/text" file.**:</span>

Commands:
- `find / -name "*.txt" -type f > /text` - to find files with the extension ".txt"
- `cat /text` - to verify the results.


## Use of `find` command to save all directories owned by user

---

> <span style="font-family:courier new">**Task 8. Find all the directories owned by user "bob" & save the output to "/bob_dir".**:</span>

Commands:
- `find / -user bob -type d > /bob_dir` - to copy directories owned by bob
- `cat /bob_dir` - to verify results


## Use of find command to list all directories & files based on UID

---

> <span style="font-family:courier new">**Task 9. Find all the directories & files owned by user w/ userid (uid) 1002 & save the output to /uid1002.**:</span>

Commands:
- `find / -uid 1002 > /uid1002` - to copy all files & directories (not just files or directories, so no `-type` option is used here) owned by user w/ uid 1002
- `cat /uid1002` - to verify the results


## Use of `tar` command to archive and compress contents of directory

---

> <span style="font-family:courier new">**Task 10. Use `tar` command to archive all contents of "/home" directory in "/root/home.tar" file.**:
>> - Compress the archived files using bzip2.</span>

Commands:
- `man tar` - saves many files together in a single tape or disk archive.
- `tar -cjvf home.tar.bz2 /home` - to archive all the contents of "/home" directory in "home.tar" file. (`-c` (create / compress), `-j` (bzip2 option); `-v` (verbose to see what's going on);  `-z` (gzip option for compression); `-f` (to specify archive file name - **must be at the end or command will fail**)). using `home.tar` since already in home dir. and specifying `bz2` since we're using "bzip2" option
- `ls -l --block-size=MB` - to display the output in MB to check again after compressing the archive.


## Use of `tar` command to extract the data from archive

---

> <span style="font-family:courier new">**Task 11. Use tar command to extract the contents of "home.tar.bz2" to "/directory".**:
>> - Delete contents of "/home" directory before extracting the data.</span>

Commands:
- `man tar` - man page for tar
- `cd /home` - pwd = home directory
- `rm -rf *` - delete all contents of "/home" dir
- `tar -xvf home.tar.bz2 -C /` - to extract (`-x`) all the contents from `home.tar.bz2` directory in `/` directory. `-C` (change to directory DIR)
- `cd /home` - change to home directory
- `ls -l` - list the contents; contents should be recovered


## Use of `tar` to archive contents with `gzip` compression

---

> <span style="font-family:courier new">**Task 12. Use `tar` command to archive all contents of "/etc" directory in "/root/etc.tar" file.**:
>> - Compress the archived files using gzip.</span>

Commands:
- `tar -czvf etc.tar.gz /etc` - to archive all the contents of "/etc" directory in "/root/etc.tar" file


## Decompressing files using gunzip & bunzip2

---

> <span style="font-family:courier new">**Task 13. Use `gunzip` command to decompress contents of "/root/etc.tar.gz" --- Use `bunzip2` command to decompress contents of "/root/home.tar.bz2"**:</span>

Commands:
- `gunzip /root/etc.tar.gz` - to decompress contents of "/root/etc.tar.gz" 
- `bunzip2 /root/etc.targ.bz2` - to decompress contents of "/root/home.tar.bz2"
- `man gunzip` - man page for gunzip
- `man bunzip2` - man page for bunzip2


## Setting `ugo-rwx` permissions on filesystems

---

> <span style="font-family:courier new">**Task 14. Create direcotry "/test" & set the user ownership to riya & Group ownership to sys.**:
>> - Remove all the premissions for others in this directory.
>> - Give full permissions at group level</span>

Commands:
- `mkdir /test` - to create directory "/test"
- `chown riya:sys /test` - to change the user ownership (riya) and group ownership (group)
- `ls -ld /test` - check ownership
- `chmod 770 /test` - to remove all permissions for others and giving full permissions at group level
- `chmod g+w,o-rx /test` - alternate method to removing permissions for others & assigning permissions as group level (for group only need to add write - for others, we need to remove read & execution; write is already not present)
- `ls -ld /test` - to verify the permissions
- `man chown` - to check man for chown, search for EXAMPLES


## Creation of symbolic links

---

> <span style="font-family:courier new">**Task 15. Create symbolic link for file "/test/sys/link/file" in "/root" directory.**:</span>

Commands:
- `cd /root` - change directory to "/root"
- `ln -s /test/sys/link/file` - to create sym link
- `ln --help` - help page for `ln`

- **NOTE** - symbolic links will point to the complete path of a file - helpful that you don't have to navigate to the entire path; you can simply type `cat file` (in home dir) and you can see the content.

**TODO**:
- [ ]  need to study the manual page for `tar` and get used to the flags
- [ ] need to study up on chmod / chown rwe permission commands