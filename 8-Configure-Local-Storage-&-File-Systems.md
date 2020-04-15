# Configure Local Storage & File Systems

## Brief Introduction to Standard Partitions
#### Types of Disks:

---

- **MBR** (Master Boot Record) - supports up to 15 partitions (probably find this disk in exam)
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
- `fdisk -l` - view disk info(s)
- `fdisk /dev/sda` - to create partition (we will create an extended partition - (**always provide disk name, not partition name**)
---
- _First input_: `n` (add new partition - **always create an "extended" partition in case there are already "3" primary; you won't be able to create another partition**), _Second input_: `e` ("extended"), selected partition (`4` in this example) & then press "Enter" twice (to assign remaining space for Logical partitions)
- Third input: `n` & press "Enter" (default first sector), Fourth input: `+2G` (the `+` is important), `wq` (to write & quit)
---
- `cat /proc/partitions` - to view partitions (kernel is not aware of the partitions just created)
- `partprobe` - to make kernel aware of this partition
- `cat /proc/partitions` - to view partitions (now you can see addt'l partitions)
---
- `mkdir /partition` - to create the mount directory
- `mkfs.xfs -f /dev/sda5` - to create xfs File System on partition (use Logical partition, not extended partition) - can use `-f` option in case fs on some partition already exists.
- `mount /dev/sda5 /partition/` - to mount partition on directory `/partition`
- `df -h` - to view mounted (not persistent mount)
- `mount` - to check the mounted File System
- `lsblk` - to list block devices
- `vi /etc/fstab` - on bottom, add `/dev/sda5` (device name) & `/partition` (mount directory) & `xfs` (file system) & `defaults 0 0` (this is not important for exam) --- (**if you make a mistake in `fstab` `mount` will let you know; you must `umount /partition/` before mounting w/ `fstab` file**)
- `mount -a` - to mount & `mount` command verifies mount

- **NOTE** - 1 GiB = 1024 MiB & 1 GB = 1000 MB

## Mounting Read-Only File System 

---

> <span style="font-family:courier new">**Task 2. Create a disk partition of size 1 GiB & mount this for read-only access on `/fat` directory**:
>> - Use `vfat` file system for the partition.
>> - Mount should be persistent.</span>

Commands:
- `fdisk /dev/sda` - to create partition (we will create a logical partition)
- _First input_: `n`, press "Enter" (default is first sector), _Second input_: `+1G`, `wq` (to write & quit)
- `partprobe` - to inform kernel about this partition
- `mkdir /fat` - to create the mount directory
- `mkfs -t vfat /dev/sda6` - to create vfat File System, on partition (`-t` = file system type)
- `mount -o ro /dev/sda6 /fat` - to mount partition on directory `/fat`
- `df -h` or `mount` - to check the mounted File System
- `lsblk` - to list block devices
--- 
mount persistently w/ `fstab` :

---
- `umount /fat` - unmount and mount persistently through `fstab`
- `vi /etc/fstab` - 
> `/dev/sda6` `/fat` `vfat` `ro 0 0` (make entry in `fstab` file - `ro` = read-only --- in the exam you will use options `0  0`)

## Configure & add Swap to your System

---

> <span style="font-family:courier new">**Task 3. Configure & add 1 GiB swap space to your system**:</span>

Commands:
- `fdisk /dev/sda` - to create partition (we will create logical partition)
- _First input_: `n`, press "Enter" (default is first sector), _Second input_: `+1G`, _Third input_: `t`, press "Enter" (for default partition), _Fourth input_: `82` (hexcode for linux swap), `wq` (write & quit)
---
- `partprobe` - to inform kernel about this partition
- `cat /proc/partition` to verify
---
- `mkswap /dev/sda7` - to configure partition as SWAP
- `vi /etc/fstab` - 
> `/dev/sda7` `swap` `swap` `defaults 0 0` (make entry in `fstab` file)
- `swapon -a` - to enable / activate SWAP as per entry in `fstab` file
- `free -m` - to verify added SWAP
- `swapon --help` - man page

## Configuring & Mounting LVMs

---

> <span style="font-family:courier new">**Task 4. Configure logical volume with name "lv_volume" which should use 200 MiB from volume group "vg_group"**:
>> - ext4 file system should be used.
>> - Mount this on `/log_vol` directory & mount should be persistent.</span>

Commands:
- `fdisk /dev/sda` - to create partition (we will create logical partition)
- _First input_: `n`, press "Enter", _Second input_: `+300M`, (*can press `l` to list all partition types*) _Third input_: `t`,(**to change partition type**) press "Enter" (for default partition), _Fourth input_: `8e` (hex code for Linux LVM), `wq` (to write & quit)
- `partprobe` - to inform the kernel about this partition
- `pvcreate /dev/sda8` - to create physical volume
- `vgcreate <insert vg_group name> /dev/sda8` - to create volume group (ex: `vg_group name` = "vg_group" per task)
- `lvcreate -n <insert lv_volume name> -L 200M vg_group` - to create logical volume on volume group (`-n` = LogicalVolume name; `-L` = LogicalVolume size) (ex: `lv_volume name` = "lv_volume" per task)
- `lvdisplay` - for more details - (**important to know `LV Path` b/c we are going to use this volume to configure the File System & for mounting purposes**)
- `mkdir /log_vol` - to create mount directory / point
- `mkfs -t ext4 /dev/vg_group/lv_volume` - to create ext4 File System for logical volume
- `mount /dev/vg_group/lv_volume/log_vol` - to mount logical volume on directory `/vol_log` - (mount volume then path for the volume, then mount point)
- `mount` - to check the mounted File System
- `lsblk` - to list block devices
- `umount /log_vol` - **must unmount prior to executing `mount -a`**
- `vi /etc/fstab` - 
> `/dev/vg_group/lv_volume` (volume group name) `/log_vol` (mount point) `ext4` (file system type) `defaults 0  0` (make entry in `fstab` file)
- `mount -a` - to mount persistently through `fstab` file

## Configuring LVMs using Physical Extents (PE) of non-default size

---

> <span style="font-family:courier new">**Task 5. Configure logical volume w/ name "volume" which should use 20 PE's from volume group "group"**:
>> - Size of PE should be a 16 MiB & File System used must be ext4 File System.
>> - Mount this on `/volume` directory & mount should be persistent.
>> - Use UUID (universal unique identifier) to mount this.</span>

Commands:
- `` - to create partition (we will create logical partition)
- _First input_: `n`, press "Enter", _Second input_: `+400M`, _Third input_: `t`, press "Enter" (for default partition), _Fourth input_: `8e` (change to Linux LVM), `wq` (to write & quit)
- `partprobe` - to inform kernel about this partition
- `pvcreate /dev/sda9` - to create physical volume
- `vgcreate -s 16M group /dev/sda9` - (`-s` = physicalextentsize) to create volume group with PE size of 16 MiB
- `vgcreate --help` - to see different options
- `lvcreate -n volume -l 20 group` - (`-n` = LogicalVolume name; `-l` = LogicalExtents Number) to create logical volume on volume group using 20 PE's (LV size = 16 * 20 = 320 MiB)
- `mkdir /volume` - to create mount directory
- `mkfs -t ext4 /dev/home/volume` - to create ext4 File System for logical volume
- `mount /dev/home/volume /volume` - to mount logical volume on directory `/volume`
- `mount` - to check the mounted File System
- `blkid` - to grab UUID
- `vi /etc/fstab`
> `UUID="1234abc..."`(UUID) `/volume` `ext4` `defaults 0  0` (make entry in `fstab`)
- `umount /volume` - unmount prior to executing `mount -a`
- `mount -a` - to mount persistently through `fstab` file

## NEW

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
- [ ]  Review Task 1.)
- [ ]  configuring & mounting LVMs Task 4.)