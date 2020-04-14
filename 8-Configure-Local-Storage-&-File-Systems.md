# Configure Local Storage & File Systems

## Brief Introduction to Standard Partitions
#### Types of Disks:

---

- **MBR** (Master Boot Record) - supports up to 15 partitions
- **GPT** (GUID Partition Table) - supports 128 partitions

- **Standard Partitions** - partitions of fixed size & size cannot be changed later.

#### Steps to configure Disk w/ Standard partitions:

---

- create the partition (`fdisk`)
- create the File System (`mkfs`)
- Mount the File System on some Directory (`mount`)

## LVMs (Logical Volume Management)

---

- Logical volumes can be extended & reduced depending on the need of time.

#### Steps to configure Disk w/ Logical Volumes:

--- 

- create the partition (`fdisk`)
- create the Physical Volume (`pvcreate`) - going to use partition for LVMs
- create the Volume Group (`vgcreate`) - by using the partition
- create the Logical Volume (`lvcreate`)
- create the File System (`mkfs`) - on the LV
- Mount the File System on some Directory (`mount`)

- Volume Groups can be extended to include new partitions in the future w/o impacting existing partitions - (`vgextend`)
- Logical volume can be extended depending on available space on volume group - (`lvextend`)

## Creating Extended & Logical Disk Partitions

---

> <span style="font-family:courier new">**Task 1. Create a disk partition of 2 GiB & mount this on `/partition` directory**:
>> - Partition should use xfs file system.
>> - Mount should be persistent.</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 
- `` - 
- `` - 
- `` - 
- `` - 
- `` - 
- `` - 

## Mounting Read-Only File System 

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![hostnamectl](images/hostnamectl.jpg)


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

**TODO**:
- [ ]  test
- [ ]  test