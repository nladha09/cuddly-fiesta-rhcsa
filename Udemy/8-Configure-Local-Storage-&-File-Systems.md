# Configure Local Storage & File Systems

## Brief Introduction to Standard Partitions
#### Types of Disks:

---

- **MBR** (Master Boot Record) - supports up to 15 partitions (**probably find this disk in exam**)
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

### LVM diagram
![lvm](/images/lvm.jpg)

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
#### creating extended partition with remaining space (no size specified)

- _First input_: `n` (add new partition - **always create an "extended" partition in case there are already "3" primary; you won't be able to create another partition**)
- _Second input_: `e` ("extended"), selected partition (`selected partition 4` in this example) & then press "Enter" twice (to assign remaining space for Logical partitions)
- should see "Partition 4 of type Extended and of size [whatev] GiB is set"

#### creating logical partition under this

- _Third input_: `n` adding logical partition (`adding logical partition 5` in this example) & press "Enter" (first sector is always default)
- _Fourth input_: `+2G` (the `+` is important), `wq` (to write & quit)
- should see "Partition 5 of type Linux and of size [2] GiB is set"

---
- `cat /proc/partitions` - to view partitions (kernel is not aware of the partitions just created)
- `partprobe` - to make kernel aware of this partition
- `cat /proc/partitions` - to view partitions (now you can see addt'l partitions)
---

#### creating file system on logical partition

- `mkfs -t xfs -f /dev/sda5` - to create xfs File System on partition (use **Logical partition**, not extended partition) - can use `-f` option in case fs on some partition already exists.
- `mkdir /partition` - to create the mount directory

---
- ~~`mount /dev/sda5 /partition/` - to mount partition on directory `/partition`~~
- ~~`df -h` - to view mounted (not persistent mount)~~
- ~~`mount` - to check the mounted File System~~
- ~~`lsblk` - to list block devices~~
---

#### mounting logical partition on `/partition` directory

- `vi /etc/fstab` - on bottom, add `/dev/sda5` (device name) & `/partition` (mount directory) & `xfs` (file system) & `defaults 0 0` (this is not important for exam) --- (**if you make a mistake in `fstab` `mount` will let you know; you must `umount /partition/` before mounting w/ `fstab` file**)
- `mount -a` - to mount & `mount` command verifies mount

- **NOTE** - 1 GiB = 1024 MiB & 1 GB = 1000 MB

## Mounting Read-Only File System 

---

> <span style="font-family:courier new">**Task 2. Create a disk partition of size 1 GiB & mount this for read-only access on `/fat` directory**:
>> - Use `vfat` file system for the partition.
>> - Mount should be persistent.</span>

Commands:

#### create logical partition

- `fdisk /dev/sda` - to create partition (we will create a logical partition)
- _First input_: `n`, press "Enter" (first sector is always default)
- _Second input_: `+1G` (define size on last sector), `wq` (to write & quit)
- `partprobe` - to inform kernel about this partition

#### create File System

- `mkfs -t vfat /dev/sda6` - to create vfat File System, on partition (`-t` = file system type) (ran into issues trying to create vfat FS - created xfs instead)

---
- ~~`mount -o ro /dev/sda6 /fat` - to mount partition on directory `/fat`~~
- ~~`df -h` or `mount` - to check the mounted File System~~
- ~~`lsblk` - to list block devices~~
- ~~`umount /fat` - unmount and mount persistently through `fstab`~~
--- 
#### mount persistently w/ `fstab` :

- `mkdir /fat` - to create the mount directory
- `vi /etc/fstab` - 
> `/dev/sda6` `/fat` `vfat` `ro 0 0` (make entry in `fstab` file - `ro` = read-only --- in the exam you will use options `0  0`)

## Configure & add Swap to your System

---

> <span style="font-family:courier new">**Task 3. Configure & add 1 GiB swap space to your system**:</span>

Commands:
#### create logical partition

- `fdisk /dev/sda` - to create partition (we will create logical partition)
- _First input_: `n`, press "Enter" (default is first sector)
- _Second input_: `+1G`
- _Third input_: `t`, press "Enter" (for default partition)
- _Fourth input_: `82` (hexcode for linux swap), `wq` (write & quit)
---
- `partprobe` - to inform kernel about this partition
- `cat /proc/partition` to verify
---

#### create SWAP :

- `mkswap /dev/sda7` - to configure partition as SWAP (don't need to configure any file system for swap)

#### mount persistently w/ `fstab`

- `vi /etc/fstab` - **no mount point - no file system for swap**
> `/dev/sda7` `swap` `swap` `defaults 0 0` (make entry in `fstab` file)
- `swapon -a` - to enable / activate SWAP as per entry in `fstab` file (**no mount needed**)
- `free -m` - to verify added SWAP
- `swapon --help` - man page

## Configuring & Mounting LVMs

---

> <span style="font-family:courier new">**Task 4. Configure logical volume with name "lv_volume" which should use 200 MiB from volume group "vg_group"**:
>> - ext4 file system should be used.
>> - Mount this on `/log_vol` directory & mount should be persistent.</span>

Commands:
#### create logical partition

- `fdisk /dev/sda` - to create partition (we will create logical partition)
- _First input_: `n`, press "Enter", _Second input_: `+300M`, (*can press `l` to list all partition types*) _Third input_: `t`,(**to change partition type**) press "Enter" (for default partition), _Fourth input_: `8e` (hex code for Linux LVM), `wq` (to write & quit)
- `partprobe` - to inform the kernel about this partition

#### create physical volume

- `pvcreate /dev/sda8` - to create physical volume on this partition

#### create volume group

- `vgcreate <insert vg_group name> /dev/sda8` - to create volume group (ex: `vg_group name` = "vg_group" per task)

#### create logical volume 

- `lvcreate -n <insert lv_volume name> -L 200M vg_group` - to create logical volume on volume group (`-n` = LogicalVolume name; `-L` = LogicalVolume size) (ex: `lv_volume name` = "lv_volume" per task)
- `lvdisplay` - for more details - (**important to know `LV Path` b/c we are going to use this volume to configure the File System & for mounting purposes**)

#### create File System for Logical Volume

- `mkfs -t ext4 /dev/vg_group/lv_volume` - to create ext4 File System for logical volume
- `mkdir /log_vol` - to create mount directory / point

---
- ~~`mount /dev/vg_group/lv_volume /log_vol` - to mount logical volume on directory `/vol_log` - (mount volume then path for the volume, then mount point)~~
- ~~`mount` - to check the mounted File System~~
- ~~`lsblk` - to list block devices~~
- ~~`umount /log_vol` - **must unmount prior to executing `mount -a`**~~
---

#### Mountinggg time
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
#### create logical partition

- `fdisk /dev/sda` - to create partition (we will create logical partition)
- _First input_: `n`, press "Enter", _Second input_: `+400M` (b/c 20 * 16 = 320) (logical volume) - need more than this to support logical volume), _Third input_: `t`, press "Enter" (for default partition), _Fourth input_: `8e` (change to Linux LVM), `wq` (to write & quit)
- `partprobe` - to inform kernel about this partition

#### create physical volume

- `pvcreate /dev/sda9` - to create physical volume

#### create volume group

- `vgcreate -s 16M group /dev/sda9` - (`-s` = physicalextentsize) to create volume group with PE size of 16 MiB
- `vgcreate --help` - to see different options

#### create logical volume

- `lvcreate -n volume -l 20 group` - (`-n` = LogicalVolume name; `-l` = LogicalExtents Number) to create logical volume on volume group using 20 PE's (LV size = 16 * 20 = 320 MiB)

#### create File System

- `mkfs -t ext4 /dev/group/volume` - to create ext4 File System for logical volume
- `mkdir /volume` - to create mount directory

---
- ~~`mount /dev/home/volume /volume` - to mount logical volume on directory `/volume`~~
- ~~`mount` - to check the mounted File System~~
---

#### mountinggg

- `blkid` - to grab UUID - even better `blkid | grep ^/dev/mapper/group-volume >> /etc/fstab`
- `vi /etc/fstab`
> `UUID="1234abc..."`(UUID) `/volume` `ext4` `defaults 0  0` (make entry in `fstab`)
- ~~`umount /volume` - unmount prior to executing `mount -a`~~
- `mount -a` - to mount persistently through `fstab` file

## Configuring Logical Volume using 100% free size Volume Group

---

> <span style="font-family:courier new">**Task 6. Configure LVM w/ name "lvm" from volume group "vgroup" of size 1GiB**:
>> - Logical volume should use complete free space on volume group.
>> - Create `ext4` file system on thie volume.</span>

Commands:
#### create logical partition

- `fdisk /dev/sda` - to create partition (we will create logical partition)
- _First input_: `n`, press "Enter", _Second input_: `+1G`, _Third input_: `t`, press "Enter" (for default partition), _Fourth input_: `8e`, `wq` (to write and quit)
- `partprobe` - to make kernel aware of partition

#### create physical volume

- `pvcreate /dev/sda11` - to create physical volume

#### create volume group

- `vgcreate <insert vg_group name> /dev/sda11` - to create volume group (ex: "vgroup" per task)

#### create logical volume

- `lvcreate --help` - to see different options
- `lvcreate -n <insert lvm name> -l 100%FREE <insert vg_group name>` - to create logical volume using all free space on volume group (ex: "lvm" = LVM name & volume group name = "vgroup" per task)
- `lvdisplay` - to view changes

#### format w/ File System

- `mkfs -t ext4 /dev/vgroup/lvm` - to create `ext4` File System for logical volume

## Extend LVMs along with File Systems

---

> <span style="font-family:courier new">**Task 7. Resize the LVM "log" so that after reboot size should be in between 217MiB to 245MiB**:
>> - Make sure complete logical volume should be usable. (also extending the File System)</span>

Commands:
- `lvdisplay` - to display logical volumes
- `lvextend --help` - to view different options available
- `lvextend -r -L +130M /dev/vg/log` - (`-r` = resizefs (**Very important to use `-r` when `lvextend`**) & `-L` = size) to extend the logical volume & resize File System

## Extending Volume Group size to increase LVM size

---

> <span style="font-family:courier new">**Task 8. Extend size of LVM w/ name "lvm" to 2 GiB**:
>> - Create new partition to increase the size of volume group.
>> - Format the complete volume e/ `ext4` File System.</span>

Commands:
- `lvdisplay` - check logical volume "lvm"
- `vgdisplay <insert name of vg_group>` - to check the size of the Volume Group (can't extend size of Logical Volume beyond the Volume Group) (ex: `vgdisplay vgroup`)

#### create logical partition

- `fdisk /dev/sda` - to create partition (we will create logical partition)
- _First input_: `n`, press "Enter", _Second input_: `+2G`, _Third input_: `t`, press "Enter", _Fourth input_: `8e`, `wq` (to write & quit)
- `partprobe` - to make kernel aware of partition

#### create physical volume

- `pvcreate /dev/sda12` - to create physical volume

#### create volume group

- `vgextend --help` - view options for `vgextend`
- `vgextend <insert vg_group name> /dev/sda12` - to extend volume group (ex: volume group name = "vgroup")

#### logical volume extend

- `lvextend -r -L +1G /dev/<insert vg_group name>/<insert LVM name>` - to extend the logical volume by 1 GiB (ex: `lvextend -r -L +1G /dev/vgroup/lvm) - `-r` is **very important to make sure to "resizefs"**

#### create File System for logical volume

- `mkfs -t ext4 /dev/<insert vg_group name>/<insert LVM name>` - to create `ext4` File System for logical volume (ex: `mkfs -t ext4 /dev/vgroup/lvm`)

## To overwrite existing File System type

---

> <span style="font-family:courier new">**Task 9. Create a standard partition of size 200 MiB & format this w/ `ext4` File System**:
>> - Change the file system to `xfs` & verify same.</span>

Commands:

#### create logical partition

- `fdisk /dev/sda` - to create partition (we will create logical partition)
- _First input_: `n`, press "Enter" (to select default first sector), _Second input_: `+200M`, `wq` (to write & quit)
- `partprobe` - to inform kernel about the partition

#### create File System

- `mkfs -t ext4 /dev/sda13` - create `ext4` File System for partition
- `mkfs -t xfs -f /dev/sda13` - force the File System change to `xfs` (overwrite)
- `blkid` - to verify


## end game LVM display
![end-game-lvm](../images/end-game-lvm.jpg)


## Configuring Directory for Group Collaboration

---

> <span style="font-family:courier new">**Task 10. Create a directory `/private/home`**:
>> - Set the user ownership to "lisa" & group ownership to "sys".
>> - Give full permissions to group "sys" on this directory.
>> - User "riya" should have no access on this directory.
>>- Add user "bob" to group "sys".
>> - Files created by user "bob" & "lara" should have group ownership to "sys".</span>

Commands:
- `mkdir -p /private/home` - to create directory
- `chown lisa:sys /private/home` - to set the user and group ownership
- `ls -ld` - verify changes
- `chmod g+rwx /private/home` - to provide full access to "sys" group
---
- `yum install acl` - to install the package for `acl` (if not already installed)
- `setfacl --help` - options for `acl` (`-m` = modify-acl, `-x` = remove-acl, `-k` = remove-default, `-d` = set default acl - applied to future files & dirs, sub-dirs created under the directory)
---
- `setfacl -R -m u:riya:- /private/home` - to remove all access for riya on this directory (`-R` for recursive, `-m` for modify, `u:` for user, `-` remove all rights)
- `setfacl - R -m d:u:riya:- /private/home` - to apply default control list so that it is applicable for  future file & sub-directories under this directory
- `getfacl /private/home` - to display `acl` applied to directory - user riya has no rights
- `usermod -aG sys bob` - to assign supplementary group to bob
- `cat /etc/group` - to check group info
---
- `chmod g+s /private/home` - to set the GID (group ID) bit on the directory (`s` is for access control list - this is called group collaboration - when files created by different group members have the group ownership set to the group name)
- `getfacl /private/home` - to display `acl` applied to directory

## Configuring ACLs on Directories

---

> <span style="font-family:courier new">**Task 11. Create a directory `/system` & configure the access as per below conditions**:
>> - User "harry" should have full access on directory.
>> - User "bob" should have read-only access on this directory.
>> - User "lisa" should have no access on this directory.
>> - Same access rules should be applicable to future files created under this directory. (default `acl`s should also be applied)</span>

Commands:
- `mkdir /system` - to create directory
- `yum install acl` - to install the package for `acl` (if not already installed)
- `setfacl -R -m u:harry:rxw /system` - to configure `acl` for harry
- `setfacl -R -m d:u:harry:rwx /system` - default `acl` for harry
- `setfacl -R -m u:bob:rx /system` - to configure `acl` for bob (**execution rights are required to move / `cd` to the directory**)
- `setfacl -R -m d:u:bob:rx /system` - default `acl` for bob
- `setfacl -R -m u:lisa- /system` - to configure `acl` for lisa
- `setfacl -R -m d:u:lisa- /system` - default `acl` for lisa
- `getfacl /system` - display `acl`s

## Mounting NFS File Systems

---

> <span style="font-family:courier new">**Task 12. Discover the NFS share exported by NFS server "server2"**:
>> - Mount the share `/nfsshare` on directory `/share` & mount should be persistent. (_everything you do in the exam should be persistent_)
>> - NFS version 3 should be used.
>> (**for the exam, only need to know about NFS client (not NFS server), how to install NFS Client, how to list NFS exports shared by NFS server & how to mount them - that's it**)</span>

Commands:
- `yum install "Network File System Client"` - to install NFS client
- `showmount -e <insert NFS server hostname>` - to discover NFS exports (ex: `showmount -e server2`)
- `mkdir /share` - to create mounting point / directory
- `mount -o nfsvers=3 <nfs server hostname>:/nfsshare/share` - (`-o` option to specify NFS version) to mount NFS export w/ NFSv3 to test if it works (ex: `mount -o nfsver=3 server2:/nfsshare/share`) (**make sure to list the entire path for File System b/c it's not present on local system, it's present on the remote NFS server**)
- `umount /share` - unmounting NFS export
- `vi /etc/fstab` - mounting persistently through `fstab` file
> `<nfs_server name>:/nfsshare` `/share` `nfs` `_netdev,nfsver=3  0  0`
> (ex: `server2:/nfsshare` `/share` `nfs` `_netdev,nfsvers=3  0  0`) - (overwrite `dev` option from defaults b/c it's a network device not present on the local system - )
- `:wq` - write & quit
- `mount -a` - to mount through `fstab` file
- `mount` - to display the mounted File Systems

## Mounting CIFS File Systems through `fstab`

---

> <span style="font-family:courier new">**Task 13. Discover the samba share & mount share "samba" on `/smb1` directory with "smb1" user**:
>> - Use the password "password" to mount this share.</span>

Commands:
- `yum install samba-client cifs-utils` - install required packages for Samba-Client (`cifs` is the File System for the Samba share)
- `smbclient -L <insert hostname for samba server>` - to discover Samba share (ex: `smbclient -L server2`) (`-L` = get a list of shares on a host)
- `mkdir /smb1` - to create mount directory

---
- ~~`mount -o username=smb1 //<insert hostname>/samba /smb1`~~ - (**for samba shares you always need to use a username while mounting the share & samba share always starts with `//` - don't forget!**) ~~to mount the samba share to test if it works~~
- ~~Enter the Samba user password: ********~~
- ~~`umount /smb1` - unmount the Samba share~~
---

- `vi /etc/fstab` - make entry in `fstab` file to make mount persistent
> `//<insert hostname>/samba`(sharename - complete path) `/smb1` `cifs` `_netdev,username=smb1,password=password  0  0`
> (ex: `//server2/samba` `/smb1` `cifs` `_netdev,username=smb1,password=password  0  0`)
- `:wq` - write & quit
- `mount -a` - to mount through `fstab` file
- `mount` - to verify the mounted file systems


**TODO**:
- [ ]  Review Task 1.)
- [ ]  configuring & mounting LVMs Task 4.)
