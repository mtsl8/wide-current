< [core system](core-sys.md)

## configuring hard drives

* [boot mode](#determining-boot-mode)
* [connected devices](#learning-about-connected-block-storage-devices)
* [allocating space](#deciding-how-to-allocate-space)
<br>__________<br>
* [erase](#erase)
* [encrypt](#encrypt)
* [partition](#partition)
* [format](#format)

### <a id=one> important context </a>

__The default path here assumes__ you are using a single free (== unused and prepared to be ERASED) HD with an __EFI system__. One could fairly easily split things up between multiple drives; concepts such as encryption, LVM, and RAID however are more complex, and will require a firm grasp of the basics.

Information about dual booting with another operating system depends on how they boot. Dual booting with Windows is possible and less complicated than it might look [here](https://wiki.archlinux.org/title/Dual_boot_with_Windows). _grub_ can manage most modern systems. rEFInd I believe is used with Mac OS for dual booting (more to come on those subjects in the future...).

___

There will be different approaches depending on:

a) the type of drive you are using:

* hard disk drives (HDD) are magnetic platter devices 
  * with virtually damageless writes to disk 
  * with limited physical durability
  * with limited data I/O speed
* solid state drives (SSD) are NAND flash memory
  * with a limited number of writes to each memory cell
  * with impressive physical durability
  * with high data I/O speed
  * with varying levels of built in drive management tools
* non-volatile memory express (NVMe) are NAND flash memory
  * SSD packaged for PCIe slots
  * with maximum I/O speed
  * with varying levels of built-in hardware encryption

and<br>
b) the type of firmware your computer has available:

* basic input output system (BIOS)
  * is the old standard firmware
  * prefers MBR partition tables, or slightly modified GPT
  * expects boot information in a specific place at the beginning of the drive
  * is not configurable via runtime software interface
  * does not provide advanced security features (just password on boot menu)
* unified extensible firmware interface (UEFI)
  * is the new standard firmware
  * prefers GPT partition tables
  * is fairly flexible and aware of bootable devices without extra help
  * can be modified and adjusted using EFI variables (efivars) at OS runtime
  * has varying levels of support for hardware based security, including 
    * secure boot
    * machine owner keys
    * trusted platform module (TPM) managment

### determining boot mode

Usually if the computer is booting in UEFI mode, the boot options listed should be labeled as such.

If you are still unsure, boot into the host, login, and run:
```
ls /sys/firmware/
```
If you do not see an "efi" folder, you are in Legacy/BIOS mode.

### learning about connected block storage devices

The command _lsblk_ will return a device tree of all available block devices.

```lsblk -f``` will include information about specific partitions, including filesystems, usage, UUID, Label.

### deciding how to allocate space

Deciding how much to allocate for what depends on what you want to do with the system.

#### boot

Using a small EFI __boot__ partition is possible as long as you store your kernels on the __root__ partition, which is wide-default. Some older UEFI implementations have issues if you make it too small - see [here](https://www.rodsbooks.com/efi-bootloaders/principles.html) - so 550M is a good safe number. If you are using BIOS/MBR you don't need a seperate __boot__ partition, but it can make things easier for encryption or for multi-booting different operating systems. In those cases, it will need to be big enough to store the bootloader & config, and any kernels to be booted. for reference, on a default efi/zen install the bootlaoder is 152K and the /boot folder is 161M.

#### swap

__swap__ partition is not mandatory but a backup for RAM and recommended on HDD's - a swap file can be used instead on SSD's (or just run swapless if you like living on the edge...). The amount to use for swap (be it a partition or file) is calculated based on available RAM and expected system usage; on older systems with less RAM, swap becomes more critical for system stability. On modern systems with more/faster RAM, usually a few GB is more than enough, __unless__ you are consistently overflowing RAM __or__ want to be able to hibernate.

* if you overflow RAM without swap in place, your system will crawl / hang / kernel panic.
* swap can also be used for hibernation, in which case you need to allocate a bit more than the amount of used RAM in order to hibernate the RAM to the hard drive. 
  * for example, 18 GB swap on 12 GB RAM is more than enough. (the old "rule" of 2x RAM no longer makes sense in most cases. 1.2-1.5 more like..)
* if you use encryption, consider the security implications of using unencrypted swap. (more info below)

keeping in mind __SSD__'s limited write cycles, in those cases it is preferable to use a [swapfile](https://wiki.archlinux.org/title/Swap#Swap_file) and the hard drive's built in [TRIM](https://wiki.archlinux.org/title/Solid_state_drive#TRIM) with [_fstrim_](https://man.archlinux.org/man/fstrim.8) to rotate the drive, rather than burn out a fixed partition<br>

#### root

The __root__ partition can be (significantly) smaller than 40 if the system is light/minimal. I usually use 100 if the drive is big because .. why not?

There are some reasons you might want to break __root__ into /var /tmp /data or the like, but that won't be covered here. Usually that is more relevant for hardening shared (server) resources to prevent casual overflow from heavy use or malicious attempts to crash the system.

#### home

__home__ at minimum needs to be at least enough to store user configuration and working files, so it's not _required_ to be very large, but it can be if you like. This is where all the files generated by your user will be kept by default. __home__ is not required to be seperate from __root__, but it does contain an entirely different context, so it is generally recommended - to have clearer awareness of drive usage, or to make managing backups easier; for example.

#### extra

one could use a seperate partition or drive for audio and yet another larger one for video. another possible use of a seperate partition would be a "vault" for example, to keep encrypted data seperately secure without complicating the rest of the system. perhaps you want to have an external drive with your music library, which you usually leave plugged in and want to be automounted on boot.. (more on mount options in the next [doc](install.md))

### [erase](erase.md) 
(optional)

### [encrypt](encrypt.md)
(optional)

### partition

NVMe devices will show up under /dev/nvme* instead of /dev/sd*. They can use multiple namespaces, each with their own partitions. The tool _nvme-cli_ can be used to manage namespaces (more info [here](https://wiki.archlinux.org/title/Solid_state_drive/NVMe).)

_gparted_ is a GTK front-end (GUI) for _parted_

_parted_ is a more scriptable and modern tool (which will be used in future iterations). 

_cfdisk_ I have tried and had issues with, perhaps others like it.

_fdisk_ is the old standard. it is interactive and provides a help menu to explain the commands available. basically, you choose a table type (MBR/GPT), create the partitions, change their type if needed, then finally write the table to the disk. for partition number and start point, usually accepting the default by pressing enter is recommended. to denote endpoint(=size), the syntax ```+500M``` or ```+100G``` (for example) is used.

replace "X" in "sdX" with the letter of the drive you are working on:
```
HD="/dev/sdX"

fdisk $HD

# or whichever partition tool you want to use..

# EFI example:
# (new GPT table)
# ...
# BOOT: 100M efi|esp
# SWAP: 4G linux swap
# ROOT: 40G linux root
# HOME: 100G linux filesystem

# BIOS example:
# (new MBR table)
# ...
# BOOT: 300M linux filesystem (optional) 
# SWAP: 8G linux swap
# ROOT: 40G linux root
# HOME: 100G linux filesystem

```
### format

EFI system partition must be FAT; FAT 16 has a limited max size so FAT 32 is usually used.

ext4 is used as a general filesystem; it is the modern linux default.

(if using BIOS/MBR with a seperate __boot__ partition, ext4 can also be used for that. FAT is simpler and therefore easier for an EFI bootloader to read; but ext4 contains built-in journaling, which can help protect filesystems during crashes)

```e2label $PARTITION $LABEL``` can be used to re-label ext filesystems after the fact.

```
# EFI:
mkfs.fat -F 32 "$HD"1
fatlabel "$HD"1 BOOT

# BIOS:
mkfs.ext4 -L BOOT "$HD"1

# both:
mkswap -L SWAP "$HD"2 
mkfs.ext4 -L ROOT "$HD"3
mkfs.ext4 -L HOME "$HD"4
```
### outro

The next step would be to mount the new partitions, but that is saved for the next [doc](install.md). One can stop here with out losing any progress, while partitions must be manually remounted each time until the FSTAB is created and the bootloader is properly set up.
