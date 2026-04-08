INSTALLING DEBIAN 13 (TRIXIE) ON A VIRTUALBOX VIRTUAL MACHINE

I went to the official website:https://www.debian.org/devel/debian-installer/


 Download debian-testing-amd64-netinst


 Open VirtualBox and create a new virtual machine

 

 

 


 And finally, please wait while it downloads.


Perfectly crafted

 

RESEARCH QUESTIONS SECTION 1

QUESTION 1: What is LUKS and what is its relationship to dm-crypt in the Linux kernel? Explain the role of the device mapper in the encryption process.

Ubuntu Manpage. (s.f.) LUKS (Linux Unified Key Setup) is the standard for disk encryption in Linux. LUKS adds a standardized header to the device's beginning, followed by a key slots area and then the data area. The complete set is called a "LUKS container." This header stores the encryption metadata and allows multiple keys to access the same device.

 

Gentoo Wiki. (2023) dm-crypt is the kernel component that performs the actual encryption. dm-crypt is a disk encryption system that uses the kernel's cryptographic API framework and the device mapper subsystem. With dm-crypt, administrators can encrypt entire disks, logical volumes, partitions, and even individual files. The dm-crypt subsystem supports the LUKS architecture, which allows the use of multiple keys to access encrypted data.

OneUptime Blog. (2026) The relationship between the two is one of complementary layers: dm-crypt is the kernel module that provides transparent disk encryption; LUKS is a disk specification and format that adds key management on top of dm-crypt; and cryptsetup is the user-space tool that manages both LUKS and dm-crypt.

Cloudflare Blog. (2025) The device mapper acts as an intermediary between the file system and the hardware. The device mapper allows pre-/post-processing of I/O requests as they travel between the file system and the underlying block device. dm-crypt, in particular, encrypts write requests before sending them to the physical device and decrypts read requests before sending them to the file system driver.

Arch Linux Wiki. (2025) Regarding the unlocking process, it maps the partitions to a new device name using the device mapper. This alerts the kernel that the device is encrypted and should be accessed via LUKS using /dev/mapper/dm_name, preventing the encrypted data from being overwritten.

 

 

 

Question 2: What is the difference between full disk encryption (FDE) and filesystem-level encryption? List two advantages and two limitations of each.

Wikipedia. (2025) Full disk encryption (FDE) operates at the block level. The term "full disk encryption" means that everything on the disk is encrypted, except for the Master Boot Record (MBR) or similar area of the boot disk. Transparent encryption, also known as on-the-fly encryption, is used by disk encryption software; data is automatically encrypted or decrypted when saved or loaded.

The Security Buddy. (2024) Filesystem-level encryption operates selectively. In filesystem-level encryption, individual files and directories within the file system are encrypted by the file system itself; the entire disk is not encrypted. Using this technology, it is possible to selectively encrypt and decrypt specific files and directories, while less sensitive ones remain unencrypted.

·         FDE advantages:

eSecurity Planet. (2022) FDE eliminates human error regarding what should be encrypted: it doesn't differentiate between sensitive and non-sensitive information, so everything is automatically encrypted by default.

Hexnode. (2023) One of the most important features of full disk encryption is that, once enabled, all data written to the disk is automatically encrypted; the decryption process also occurs automatically when reading data.

 

 

 

·         FDE limitations:

Bitdefender. (2025) FDE is most effective when the system is powered off. Once the system boots and is decrypted, the data becomes accessible and vulnerable; FDE alone cannot prevent data theft during active sessions.

Wikipedia. (2025) FDE is also vulnerable when a computer is stolen while in suspended mode, since resuming does not involve a BIOS boot sequence and typically does not prompt for the encryption password.

·         File-level encryption advantages:

Bitdefender. (2025) File-level encryption keeps individual files encrypted even while the system is running, making stolen data unusable to attackers.

Hexnode. (2023) Encrypted files sent to other devices remain encrypted until the password or encryption key is entered, thus protecting data both at rest and in transit.

·         File-level encryption limitations:

eSecurity Planet. (2022) Managing numerous encryption keys is not always practical. This drawback can be alleviated with key management tools, but that involves an additional tool or service that requires money and maintenance effort.

File-level encryption addresses a very limited set of threats; it lacks safeguards against advanced persistent threats (APTs), malicious insiders, or external attackers.

 

 

QUESTION 3: What is LVM and what are its three layers of abstraction (PV, VG, LV)? What advantages does LVM offer over traditional partitioning in a server environment?

Red Hat. (s.f.) LVM (Logical Volume Manager) introduces an abstraction layer between physical disks and the operating system. LVM creates an abstraction layer over physical storage, allowing the creation of logical storage volumes. This offers greater flexibility compared to directly using physical storage. Furthermore, the storage hardware configuration is hidden from the software, so it can be resized and moved without stopping applications or unmounting file systems.

ü  The three layers of abstraction:

DigitalOcean. (2022) Physical Volumes (PV): PVs are actual physical block devices or other disk-like devices that LVM uses as raw material for the higher levels of abstraction. LVM writes a header to the device to assign it for management.

DigitalOcean. (2022) Volume Groups (VGs): LVM combines physical volumes into storage groups called volume groups. VGs abstract the characteristics of the underlying devices and function as a single, unified logical device with the combined storage capacity of the PVs that comprise it.

DigitalOcean. (2022) Logical Volumes (LVs): A volume group can be divided into any number of logical volumes. LVs function like traditional partitions but with the significant advantage that they can be dynamically resized.

 

 

 

ü  Advantages over traditional server partitioning:

Traditional partitioning is rigid: once the size of a partition is defined, modifying it is a complex and risky operation. LVM allows for hot resizing of volumes (often with no downtime), aggregating multiple disks into a single storage pool, and creating instant snapshots.

Red Hat. (s.f.) Additionally, with LVM it's possible to move data while the system is running using the pvmove command; data can be reorganized on disks while they are in use, for example, to clear a hot-swappable disk before removing it.

QUESTION 4: Explain what happens during the boot process of a LUKS+LVM system: at what point is the decryption password requested, and what role does GRUB play in this process?

Gentoo Wiki. (2025) The boot process of a LUKS+LVM system follows a precise sequence with multiple components: Without encryption, the boot process is already very complex. Adding encryption to the root filesystem increases this complexity considerably because some mechanism must decrypt the root filesystem so that the kernel can start the init process. An early boot environment—such as initramfs—is typically used to perform disk decryption during the boot process.

ü  Specific sequence is:

lvm-on-luks (2022) GRUB is loaded first, from the (unencrypted) /boot partition. Since /boot can be an LVM on top of LUKS, the LUKS container must first be decrypted and loaded by GRUB; this occurs with the LUKS password before the GRUB menu appears.

 

lvm-on-luks (2022) The password is requested during the initramfs phase, before mounting the root system. Systemd continues the process: it unlocks the LUKS partition, activates LVM, mounts the actual root filesystem, destroys the initramfs, and transfers control of init to the actual root system.

Gentoo Wiki. (2023) The initramfs orchestrates the LVM decryption. A very basic initramfs might contain only tools like mount, cryptsetup, and switch_root. First, /dev, /sys, and /proc are mounted; then cryptsetup is used to open and map the encrypted drive; finally, it is mounted, and switch_root switches to the new root filesystem to start the init process.

Bibliographic References

Arch Linux Wiki. (2025). Dm-crypt/Device encryption. https://wiki.archlinux.org/title/Dm-crypt/Device_encryption

Arch Linux Wiki. (2026). Dm-crypt/Encrypting an entire system. https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system

Archont00. (s.f.). Arch Linux LUKS TPM boot: A guide for setting up LUKS boot with a key from TPM [GitHub Repository]. https://github.com/archont00/arch-linux-luks-tpm-boot

Bitdefender. (2025). What is full disk encryption (FDE) & how it works. https://www.bitdefender.com/en-us/business/infozone/what-is-full-disk-encryption-fde

Cloudflare Blog. (2025). Speeding up Linux disk encryption. https://blog.cloudflare.com/speeding-up-linux-disk-encryption/

Cryptosetup Team. (2019). Full disk encryption, including /boot: Unlocking LUKS devices from GRUB [Debian Documentation]. https://cryptsetup-team.pages.debian.net/cryptsetup/encrypted-boot.html

DigitalOcean. (2022). An introduction to LVM concepts, terminology, and operations. https://www.digitalocean.com/community/tutorials/an-introduction-to-lvm-concepts-terminology-and-operations

eSecurity Planet. (2022). Disk vs file encryption: Which is best for you? https://www.esecurityplanet.com/threats/disk-vs-file-encryption-which-is-best-for-you/ 

Gentoo Wiki. (2023). Full disk encryption from scratch simplified. https://wiki.gentoo.org/wiki/Full_Disk_Encryption_From_Scratch_Simplified

Gentoo Wiki. (2025). Dm-crypt. https://wiki.gentoo.org/wiki/Dm-crypt

Hexnode. (2023). File-based encryption vs full-disk encryption. https://www.hexnode.com/blogs/file-based-encryption-vs-full-disk-encryption/

The Linux Kernel Organization. (s.f.). Device-Mapper's "crypt" target [Kernel Documentation v5.13]. https://www.kernel.org/doc/html/v5.13/admin-guide/device-mapper/dm-crypt.html

OneUptime Blog. (2026). How to configure dm-crypt and LUKS for block device encryption on Ubuntu. https://oneuptime.com/blog/post/2026-03-02-configure-dm-crypt-luks-block-device-encryption-ubuntu/view

 

rdkr. (s.f.). lvm-on-luks: Set up full partition encryption for Ubuntu using LUKS and GRUB2 [GitHub Repository]. https://github.com/rdkr/lvm-on-luks

Red Hat. (s.f.). Chapter 1: Overview of logical volume management. In Configuring and managing logical volumes — Red Hat Enterprise Linux 9. https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_and_managing_logical_volumes/overview-of-logical-volume-management_configuring-and-managing-logical-volumes

Red Hat. (s.f.). Configuring and managing logical volumes — Red Hat Enterprise Linux 8. https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html-single/configuring_and_managing_logical_volumes/index

Red Hat. (2025). Logical Volume Manager (LVM) versus standard partitioning in Linux. https://www.redhat.com/sysadmin/lvm-vs-partitioning

The Security Buddy. (2024). Full disk encryption and file system level encryption. https://www.thesecuritybuddy.com/encryption/full-disk-encryption-and-filesystem-level-encryption/

Ubuntu Manpage. (s.f.). cryptsetup — manage plain dm-crypt and LUKS encrypted volumes [Man page]. https://manpages.ubuntu.com/manpages/jammy/man8/cryptsetup.8.html


lvm-on-luks (2022) https://github.com/rdkr/lvm-on-luks

 
