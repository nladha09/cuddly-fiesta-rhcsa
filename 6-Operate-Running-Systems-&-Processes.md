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

## Installing KVM using virt-manager

---

> <span style="font-family:courier new">**Task 4. Install RedHat / CentOS 7 on system as guest OS using KVM hypervisor (very unlikely this will be on exam).**:
>> - Virtual machine must be started at boot process.</span>

Pre-requisites: (when using Linux as host operating system)
- host machine CPU must have 64-bit architecture
- virtualization must be enabled from BIOS setting of machine
- check `cat /proc/cpuinfo | grep vmx` (check for vmx component - for intel processors)


Commands:
- `yum groups install "Virtual Host"` or `yum install virt-manager qemu-kvm` - to install required packages.
- `systemctl status libvirtd` - to check the status of `libvirtd` (daemon - sits b/w virtualization layer & administrative layer to access kvm).
- `virt-manager` - (starts install of kvm) then follow the installation sequence.
- `virsh list --all` - to list all KVMs running on host machine.
- `vrish start <insert machine_name>` - to start the machine console.
- `virsh reboot <insert machine_name>` - to reboot the machine.

## Tuning Kernel Interface parameter vm.swappiness persistently

---

> <span style="font-family:courier new">**Task 5. Modify kernel parameter `vm.swapiness` to set the value to 10.**:
>> - Changes done should persist after reboot.</span>

- Definition: `vm.swappiness` - the default value of `vm.swappiness` is 60 & represents the percentage of the free memory before activating swap. The lower the value, the less swapping is used & the more memory pages are kept in physical memory.

- Changing the value directly influences the performance of the Linux system.
These values are defined:
* 0: swap is disabled
* 1: minimum amount of swapping w/o disabling it entirely
* 10: recommended value to improve performance when sufficient memory exists in a system
* 100: aggressive swapping

Commands:
- `sysctl -a | grep swappiness` - to display _all_ existing values of this variable.
- `sysctl -w vm.swappiness` - to change (_write_) the parameter _only_ during runtime (keep in mind after a reboot, changes done will be lost) - **dont' use this in the exam - everything will need to persist in the exam**
- for persistent change: `vi /etc/sysctl.conf` & add `vm.swappiness=10` at the bottom of file & `:wq` 
- after reboot, new value will be effective.

## Disable IPv4 IP Forwarding using `sysctl.conf`

---

> <span style="font-family:courier new">**Task 6. Modify kernel parameter to disable ipv4 packet forwarding.**:
>> - Changes done should persist after reboot.</span>

Definition:
IP forwarding allows an operating system (here on Linux) to forward packets as a router does or more generally to route them through other networks.

Commands:
- `sysctl -w net.ipv4.ip_forward=0` - to change the parameter in runtime, by default the parameter is set to `1` (meaning it is enabled). (**Keep in mind after reboot changes done will be lost**)
- for persistent change: `vi /etc/sysctl.conf` & `net.ipv4.ip_forward=0` & `:wq`
- after reboot, new value will be effective.

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


## NEW

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
