## Starting with Important Tasks

##### Installing RHEL 8 VM on VMware Workstation (RHEL 8)

> <span style="font-family:courier new">**Task 1. Install RHEL 8 VM on VMware workstation using image file with disk space allocation as below**:
>> Total Disk Space (Max Disk Size) - 40 GB\
"/" - 15 GB\
"/home" - 10 GB\
"/boot" - 300 MB\
RAM Memory - 4GB</span>

Procedure (Building VM):
- Open VMware Workstation software & click on "Create a New Virtual Machine".
- Click "Next" & browse the OS Image File from your local storage.
- Give VM a name (i.e. "SYSTEM") & then click "Next".
- Give Max disk space = 40 GB & click "Next".
- Customize hardware settings for Memory / RAM (4096 MB = 4GB) & click "Finish".
- Before starting up the VM (this is a VMware specific step) go to "Edit virtual machine settings" delete the "SCSI" disk type and "Create a new virtual disk" select either "IDE" or "SATA" (use IDE & follow steps for disk space sizing).
- Click on "Power on this virtual machine" - if you run into the below error:
> VMware Workstation and Device/Credential Guard are not compatible. VMware Workstation can be run after disabling Device/Credential Guard. Please visit http://www.vmware.com/go/turnoff_CG_DG for more details.

- run this command as Administrator: `bcdedit /set hypervisorlaunchtype off` & restart your host

Procedure (RHEL 8 Installation set up): 
- Select your language "English"








##### Interrupt the boot process to set the root password (to gain access)

##### Set the SELinux to Enforcing mode

##### Verifying SELinux Status

##### Starting with Important Tasks (Tasks in PDF File)

