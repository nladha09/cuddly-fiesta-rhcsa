# Managing Layered Storage & Configuring Disk Compression

## Brief Intro - Managing Layered Storage using Stratis

---

![stratis-storage](images/stratis-storage.jpg)

- Stratis is a Linux local storage management tool that aims to enable easy use of advanced storage features such as:
    - thin provisioning
    - snapshots of File System
    - pool-based management
    - monitoring

- It is a hybrid user-&-kernel local storage management system that uses the concept of a storage pool. Only focused on the _User Layer_ for the exam.

- This pool is created from one or more local disks or partitions, & volumes got create from the pool. The only thing we need to take care of is the Storage pool; should not run out of storage, if it is, we must add more block devices to storage pool.

- Stratis can be used on top of any storage device (including LVM)

- The File System is put on top of the volume and is an integrated part of it; that means when volume size gets increased, the File System is also resized automatically.


## Creating Stratis Pool and File Systems

---

> <span style="font-family:courier new">**Task 1. Create a Stratis pool using two partitions each of 1 GiB**:
>> - Name the pool as "firstpool".
>> - Create two File Systems `data` & `logs` on pool.
>> - Mount the Stratis File Systems `data` & `logs` on directories `/data` & `/logs` respectively (through `fstab`)</span>

Commands:
- `yum install stratisd stratis-cli` - install `stratisd` daemon & `stratis-cli` packages
- `systemctl enable --now stratisd` - to start & enable `stratisd` (**remember this otherwise it will not work**)
- `fdisk /dev/sda` - `n` - `+1G` - create three partitions (extended partition #4 & logical partitions 5,6,7, each of 1G)
- `stratis pool create <pool_name_firstpool> /dev/sda5 /dev/sda6` - to create the Stratis pool using two partitions
- `stratis filesystem create <pool_name_firstpool> <insert_fs_name_data>` - to create the File System with name `data` on Stratis pool
- `stratis filesystem create <pool_name_firstpool> <insert_fs_name_logs>` - to create the File System with name `logs` on Stratis pool
- `stratis pool list` - to list Stratis pool(s)
- `stratis filesystem list` - to list Stratis File Systems
- `mkdir /data /logs` - to create the mount points
- `vi /etc/fstab` - to create entries for File Systems in `fstab` for persistent mount
> - `/stratis/firstpool/data` `/data` `xfs` `defaults  0  0`
> - `/stratis/firstpool/logs` `/logs` `xfs` `defaults  0  0`
> - `:wq`
- `mount -a` - to mount the File Systems through `fstab`
- `df -h` - 
- `man stratis` - to check the manual page for stratis

## Extending Stratis Storage pool adding more devices

---

> <span style="font-family:courier new">**Task 2. Extend the Stratis pool `firstpool` by adding one more partition of size 1 GiB**:</span>

Commands:
- `stratis pool add-data <pool_name_firstpool> /dev/sda7` - to extend the size of stratis storage pool adding one more partition
- `stratis pool list` - to list stratis pool(s)
- `stratis filesystem list` - to list stratis File Systems
---
- **NOTE** - to delete the stratis pool you need to delete all File Systems first & only then will you be allowed to delete the pool.
---
- `stratis filesystem destroy <pool_name> <filesystem_name>` - to delete stratis File System
- `stratis pool destroy <pool_name>` - to delete stratis storage pool
- `man stratis` - to check manual page stratis 

## Creating Stratis File System Snapshots

---

> <span style="font-family:courier new">**Task 3. Create snapshot for Stratis File System `data` & name the snapshot as `data_snapshot`**:
>> - Mount the `data_snapshot` File System on `/data_snap` directory.</span>

Commands:
- `stratis fs snapshot <pool_name_firstpool> <origin_name_fs_data> <snapshot_name_data_snapshot>` - to create snapshot of File System `data` (this can take ~20 seconds)
- `stratis pool list` - to list stratis pool(s)
- `stratis filesystem list` - to list stratis File Systems
- `mkdir /data_snap` - create mounting point
- `vi /etc/fstab` - to create entries for File Systems in `fstab` for persistent mount
> `/stratis/firstpool/data_snapshot` or `UUID` from `blkid` `/data_snap` `xfs` `defaults  0  0`
- `mount -a` - to mount the File Systems through `fstab`
- `man stratis` - to check manual page for stratis

## Renaming Stratis Pool & File Systems

---

> <span style="font-family:courier new">**Task 4. Rename stratis pool `firstpool` as `newpool` & stratis File System `data` as `newdata`**:</span>

Commands:
- `stratis pool list` - to list stratis pool(s) & verify
- `stratis pool rename firstpool newpool` - to rename pool
- `stratis filesystem rename <pool_name_newpool> <fs_name_data> <new_name_newdata>` - to rename filesystem
- `stratis filesystem list` - to list stratis File Systems & verify

- **NOTE** - previous device names in `fstab` file that used `firstpool` are no longer valid now... you must change this now & remount using `mount -a`. Best practice is to use UUID so that there is no future impact.
 
## Brief Intro to Virtual Data Optimizer (VDO)

---



## NEW

---

> <span style="font-family:courier new">**Task 2. **:</span>

Commands:
- `` - 
- `` - 
- `` - 
- `` - 

![hostnamectl](images/hostnamectl.jpg)


## NEW

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