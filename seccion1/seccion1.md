Section 1: Installing Debian 13 with LVM and disk encryption
 We loaded the Debian 13 ISO image into our virtual machine (VirualBox).
Note: In this image we can see the addresses from where the ISO image is extracted and where the data will be stored.
Similarly, we add the necessary resources that we will use in our operating system.
4gb of RAM
4 cores of CPU
20gb storage
 	We select graphical install which is the interface. 

We entered the hostname of the “debian-B1” system.
 We created the superuser (root) password.


We configured the LVM partitioning scheme with disk encryption by selecting the option: "Guided - use entire disk and set up encrypted LVM".



 Select the disk on which the partition will be performed.
SCS13 (0,0,0)(sda) – 21.5GB ATA VBOX HARDDISK

 In the partitioning scheme, we select the option -separate /home partition-. This choice ensures that the installer automatically creates the logical volumes we need: root(/), swap, and home. 

Here, the installer asks for permission to write the basic partition table to the virtual disk before configuring the LVM layer and encryption.
Select "yes," and the /boot partition (outside of LVM) and the partition containing the LUKS encrypted volume, where your logical volumes will be stored, will be created.





We open KeePassXC and set a LUKS encryption password with 100-bit entropy.

 We used the LUKS encryption password to encrypt the disk partition








 We assign the maximum disk volume (20.5GB) for partitioning

 In this section we note the storage hierarchy and encryption over LVM with the root(/), swap, /home volumes
Finally, completed the installation and can now use our Debian 13 operating system








At the end of the installation, it will ask for our 100-bit password
note: this image means that LUKS encryption is active.
We log into the Debian 13 terminal and are ready to work






First command: lsbk
lsblk (list block devices)lists information about all available or the specified block
       devices. The lsblk command reads the sysfs filesystem and udev db
       to gather information (lsblk(8) - Linux manual page, s/f).
result
sda (disk, 20G): It is the main physical hard drive (20 GB virtual drive).
sda1 (part, 956M): First physical partition. It is mounted at /boot, which is where the kernel resides to boot the system.
sda2 (part, 1K): It's an extended partition (a "container" for other partitions). Its 1K size is just a placeholder.
sda5 (part, 19.1G): It is a logical partition within sda2.
sda5_crypt (crypt, 19G): It indicates that the sda5 partition is encrypted (probably using LUKS). Everything that follows is inside this secure "container".
debian--B1--vg-root (lvm, 7.5G): An LVM logical volume where the root operating system is installed (/).
debian--B1--vg-swap_1 (lvm, 1G): System swap space (virtual memory).
debian--B1--vg-home (lvm, 10.5G): Logical volume for users' personal files, mounted at /home.
sr0 (rom, 1024M): It represents the CD/DVD-ROM drive (in this case, probably an ISO image mounted on the virtual machine).
Sda: The sda ​​device is the physical disk; this disk is partitioned into 3 structures (sda1, sda2 and sda5) which are the MBR partition table standard (The Linux Kernel documentation, s/f, b8)
note: In this image we observe the output of the lsblk command
second command: pvs
	Physical Volume Scan (pvs) Its function is to report on the Physical Volumes (PV) that are being used by LVM (Logical Volume Manager) (LVM - Debian Wiki, s/f).
PV (Physical Volume): /dev/mapper/sda5_crypt: evidence that for LVM the physical disk is the encryption container. 
VG (Volume Group): debian-B1-vg: It is the storage container where logical partitions (root, home, swap) are created.
Fmt (Format): <19.05g: This is the total size detected within the encryption.
PFree: 0 (PFree): Indicates that there is no free space in the volume group; all of the 19 GB space has already been allocated between your system partitions (LVM - Debian Wiki, s/f).
 Note: The second command, ‘pvs’, requests root (superuser) permissions. We log in as root and execute the command.
Third command: lvs
	Logical Volume Scan (lvs), this command lists and reports on the properties of Logical Volumes (LV) (LVM - Debian Wiki, s/f).
Attr (Attributes): Una cadena de atributos (en tu caso -wi-ao----) (LVM - Debian Wiki, s/f).
home (10.51g): This is the volume allocated for user data. It has the attribute "o", which means it is mounted at "/home".
root (<7.53g): This is the volume where the operating system resides. The "<" symbol indicates that the size is approximate based on block alignment. It is mounted at /.
swap_1 (<1.01g): This is the swap space. It is used when physical RAM is exhausted (LVM - Debian Wiki, s/f).
Note: The allocation of sizes and state attributes for the fundamental partitions of the operating system is detailed.



FOURTH COMMAND: vgs
	 Volume Group Scan (vgs) reports information about the system's volume groups (LVM - Debian Wiki, s/f).
VG (Volume Group): The name assigned to the group (debian-B1-vg).
PV (Number of Physical Volumes): How many physical disks make up this group. In this case, it's 1 (the encrypted partition sda5_crypt).
LV (Number of Logical Volumes): How many logical partitions it has. In this case, there are 3 (root, home, and swap_1).
SN (Number of Snapshots): Security snapshots. We have 0.
Attr (Attributes): Group attributes (wz--n-), Writable (w), Resizable (z)
VSize (Volume Size): The total available size in the group (19.05 GB).
VFree (Volume Free): Unallocated free space. We have 0, which means we've already used the entire disk (LVM - Debian Wiki, s/f).
Note: The output of the vgs command confirms the consolidation of storage in the debian-B1-vg group.






fifth command: cryptsetup status
Cryptsetup is used to conveniently setup dm-crypt managed device-mapper mappings. These include plain dm-crypt volumes and LUKS volumes. The difference is that LUKS uses a metadata header and can hence offer more features than plain dm-crypt. On the other hand, the header is visible and vulnerable to damage. 
STATUS: Reports the status for the mapping (cryptsetup(8) — cryptsetup-bin — Debian trixie — Debian Manpages, s/f).
Encrypted type: LUK2: LUKS2 is the next-generation disk encryption format for Linux. It introduces critical improvements such as metadata redundancy (protection against corruption), support for new key derivation algorithms, and greater configuration flexibility (Fruhwirth & Rajic, s.f.)
Algorithm (Cipher): aes-xts-plain64: It is the mathematical core that encrypts your files.
Key Size: 512 bits: This is the length of the encryption key.
Physical Structure Parameters
Device: /dev/sda5: Confirms that the encryption resides physically on the fifth partition of the first disk.
Sector size: 512: This is the size of the basic data block.
Offset: 32768 sectors: This is the space at the beginning of the disk where the LUKS2 headers (metadata) are stored before your actual data begins.
Mode: read/write: Indicates that the volume is open and you can write or read data from it.

Note: Detailed audit of the cryptographic container using cryptsetup. The use of the LUKS2 standard with a 512-bit AES-XTS algorithm is confirmed.
Research Questions:
1. What is LUKS and what is its relationship to dm-crypt in the Linux kernel? Explain the role of the device mapper in the encryption process.
LUKS (Linux Unified Key Setup) is a disk encryption standard that defines a header format for encrypted data (Cryptsetup, n.d.).
dm-crypt is the transparent disk encryption subsystem of the Linux kernel (dm-crypt/Encrypting an entire system, n.d.).
Dm-crypt provides block-level encryption but lacks its own header format; LUKS fills this gap by storing the configuration information necessary for the kernel to know how to decrypt the data.
2. What is the difference between full disk encryption (FDE) and filesystem-level encryption (FDI)? List two advantages and two limitations of each.
Full disk encryption (FDE) is a data protection method that encrypts every bit of data on a hard drive using disk encryption software or hardware 


Advantages
Better Data Security. One of the primary advantages is the increased protection against unauthorized access to sensitive information stored on encrypted hard drives.
Seamless User Experience. The user-friendly nature of FDE makes it an attractive option for organizations looking to implement strong security measures without disrupting daily operations (What is FDE and How it Works? - Bitdefender InfoZone, 2025).
Limitations:
FDE Can Slow Processes: A shortcoming of full disk encryption is that it can hinder productivity. Any form of encryption can make it take longer to access data, as devices must decrypt a file to read it. This isn’t an issue most of the time, given the power of modern devices, but since FDE also encrypts the operating system, it can slow the system.
FDE Doesn’t Apply When Files Are in Use: Just as full disk encryption doesn’t encrypt data in transit, it doesn’t protect files currently in use, either. When an authorized user opens an FDE-encrypted file, they decrypt it, and it encrypts again once they log out. That means this data could be vulnerable while users are working with it (Matthews, 2025).
File System Level Encryption
fscrypt is a library which filesystems can hook into to support transparent encryption of files and directories.


Advantages:
Granularity: Allows different users on the same machine to have their own keys, so one user cannot see another's files even when the system is on.
Backup flexibility: Encrypted files can be backed up individually to the cloud or external drives while maintaining their original encryption.
Limitations:
Metadata leakage: Attackers can still see how many files there are, their names (in some cases), and when they were modified, revealing usage patterns.
Exposed temporary files: Sensitive data can be left in plain text on swap partitions or temporary directories (/tmp) if these are not encrypted separately (Filesystem-level encryption (fscrypt) — The Linux Kernel documentation, s/f).
3. What is LVM and what are its three layers of abstraction (PV, VG, LV)? What advantages does LVM offer over traditional partitioning in a server environment?
Logical volume management with LVM provides a more flexible way to manage the disk space on a system than the traditional approach of dividing a disk into partitions. LVM gives the system administrator much more flexibility in allocating storage, because partitions controlled by LVM be resized and moved around, and a single partition can reside on more than one physical hard disk.
The three layers of abstraction:
Physical volumes: correspond to disks. They represent the lowest abstraction level of LVM, and are used to create a volume group.
Volume groups: are collections of physical volumes. They are pools of disk space that logical volumes can be allocated from.
Logical volumes: correspond to partitions – they usually hold a filesystem. Unlike partitions though, they can span multiple disks (because of the way volume groups are organized) and do not have to be physically contiguous (About logical Volume Management (LVM), s/f)
Advantages over traditional partitioning
Hot resizing: Unlike fixed partitions, a volume volume (LV) can be extended while the system is running if the volume group (VG) has space, avoiding downtime.
Snapshots: LVM allows you to create read-only or read/write copies of a volume at a given point in time, greatly facilitating consistent backups without stopping the database or service.
4. Explain what happens during the boot process of a system with LUKS+LVM: at what point is the decryption password requested, and what role does GRUB play in this process?
The boot process of a system that combines LUKS and LVM is a sequence of "layers" where each layer must unlock the next so the kernel can access the data.
The LUKS+LVM boot process
Loading the Boot Manager (GRUB): The BIOS/UEFI loads GRUB from an unencrypted partition (usually /boot).
Loading the Kernel and initramfs: GRUB loads the Linux kernel and the temporary filesystem called initramfs (or initrd) into RAM.
Password Prompt: The cryptsetup binaries reside within initramfs. The system pauses and prompts for the password to open the LUKS container.
Activating LVM: Once dm-crypt decrypts the device, the kernel detects that an LVM Physical Volume (PV) is present. At that point, the Volume Groups (VG) are scanned and the Logical Volumes (LV) (such as /root or /home) are mounted to continue booting (dm-crypt/System configuration, s/f)
GRUB supports LUKS device decryption, allowing even the /boot directory to be protected, although this requires the user to enter the key twice (once in GRUB and once in the operating system) unless a key file is used (GNU GRUB Manual 2.14, s. f.).
Full Encryption (including /boot): In this scenario, GRUB is the one that requests the first password in order to read its own configuration files and the kernel.