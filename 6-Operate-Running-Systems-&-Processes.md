# Operate Running Systems & Processes

## Adjusting priority of Process w/ `renice`

---

> <span style="font-family:courier new">**Task 1. CPU intensive Process w/ name `dd` is running on system w/ NICE (NI) value of -5 & taking more CPU attention than default.**:
>> - Adjust the niceness value to 5 so that CPU pays less attention to this process.</span>

Commands:
- `top` - to check the nice (NI) value & priority (like task manager)
- `renice -n 5 -p <insert PID #>` - to adjust nice value (`-p` = process)

NICE value can be between -20 to 19. The lesser the (NI) value, the more CPU resources will be used. The higher the (NI) value, less CPU attention will be given. The higher value of priority (PR) means less priority.

- **NOTE** - never run process w/ (NI) value of -20, CPU will give highest prioirty & no other jobs will be able to run.

## To run job w/ a pre-defined (NI) value

---

> <span style="font-family:courier new">**Task 2. Run the below command in the background w/ (NI) value of 10.**:
>> - `sleep 3600`</span>

Commands:
- `nice -n 10 sleep 3600 &` - to start a process with a pre-defined (NI) value. `&` runs the task in the background.

## Killing a Process forcefully

---

> <span style="font-family:courier new">**Task 3. Kill the process `dd` to stop forcefully.**:</span>

Commands:
- `kill -9 <insert PID #>` - to kill the process `dd` forcefully. (`-15` would stop the task gracefully)

- another way - after typing `top` hit `k` and type in "PID #".

## Use of `grep` for non-matching pattern (invert-match)

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![hostnamectl](images/hostnamectl.jpg)


## Use `find` command to list files owned by user

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![hostnamectl](images/hostnamectl.jpg)


## Using `find` command to locate file using name of file

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![hostnamectl](images/hostnamectl.jpg)


## Finding files with extension .txt using `find` command

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![hostnamectl](images/hostnamectl.jpg)


## Use of `find` command to save all directories owned by user

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![hostnamectl](images/hostnamectl.jpg)


## Use of find command to list all directories & files based on UID

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![hostnamectl](images/hostnamectl.jpg)


## Use of `tar` command to archive and compress contents of directory

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![hostnamectl](images/hostnamectl.jpg)


## Use of `tar` command to extract the data from archive

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![hostnamectl](images/hostnamectl.jpg)


## Use of `tar` to archive contents with `gzip` compression

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![hostnamectl](images/hostnamectl.jpg)


## Decompressing files using gunzip & bunzip2

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![hostnamectl](images/hostnamectl.jpg)


## Setting `ugo-rwx` permissions on filesystems

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![hostnamectl](images/hostnamectl.jpg)


## Creation of symbolic links

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![hostnamectl](images/hostnamectl.jpg)
