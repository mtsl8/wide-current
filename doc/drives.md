### important context

information about dual booting with another os depends on how they boot. dual booting with Windows is possible and less complicated than it might look [here](https://wiki.archlinux.org/title/Dual_boot_with_Windows). grub can manage most modern systems. rEFInd I believe is used with Mac OS for dual booting. more to come on those subjects in the future...

the default path here assumes you are using a single free (== unused and prepared to be ERASED) HD; however you could fairly easily split things up between multiple drives. concepts such as encryption, LVM, and RAID are more complex, and will require a firm grasp of the basics.

there will be different approaches depending on:

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
  * packaged for PCIe slots
  * with maximum I/O speed
  * with varying levels of built in hardware encryption

and<br>
b) the type of firmware your computer has available:

* basic input output system (BIOS)
  * is the old standard firmware
  * prefers MBR partition tables, or slightly modified GPT
  * expects boot information in a specific place at the beginning of the drive
  * is not configurable via runtime software interface
  * does not provide many security features
* unified extensible firmware interface (UEFI)
  * is the new standard firmware
  * prefers GPT partition tables
  * is fairly flexible and aware of bootable devices without extra help
  * can be modified and adjusted using EFI variables (efivars) at runtime
  * has varying levels of support for hardware based security, including 
    * secure boot
    * machine owner keys
    * trusted platform module (TPM) managment

### deciding how to allocate space

deciding how much to allocate for what depends on what you want to do with the system.

using a small efi boot partition is possible as long as you store your kernels on the root partition, which is wide-default. <br>

swap partition is not mandatory but a backup for RAM - can can use a swap file instead (or none if you like)<br>
if you overflow RAM without swap in place, your system will either slow to a crawl or hang / kernel panic <br>
swap can also be used for hibernation, in which case you need a bit more than used RAM to snooze. <br>

the root partition can be (significantly) smaller than 40 if the system is light. <br>
I usually use 100 if the drive is big because why not. <br>

home at minimum needs to be at least enough to store user configuration and working files, so not that big really <br>
one could use a seperate partition or drive for audio and yet another larger one for video.. for example. <br>

however you want to set it up, once you get it there, you can lock it in to the [FSTAB](https://wiki.archlinux.org/title/Fstab) for automount. <br>
(assuming the initramfs and/or udev are configured to load all the modules required to access them ~ such as for encryption)

### partitioning the drive(s)


example partitions:
_(replace "X" with the letter of the drive you are installing to:)_
```

lsblk -f

HD="/dev/sdX"

fdisk $HD

# or whichever partition tool you want to use..
# (new GPT table)
# ...
# BOOT: 100M +efi/esp/boot flag
# SWAP: 4G linux swap
# ROOT: 40G linux root
# HOME: 100G linux filesystem
```
### erase, encrypt ~ incomplete, optional

Encryption ontop of all 1's or straight 0's is not very strong. encryption on random is.<br>
one can use /dev/urandom alone but it's a lot faster to use openssl

NAND memory (flash, SSD, NVMe) does not enjoy being bit blasted; it reduces their lifespan <br>
more sophisticated devices have built in harware encryption made for the drive. <br>
buy those / use that !! not actually too pricey for primo NVMe

HDD's do not mind at all, but it will take a while.

these block settings were the fastest I found on my 7200rpm seagate HDD
```
OBLK=1024K
IBLK=64K

PASS=$(tr -cd '[:alnum:]' < /dev/urandom 2>/dev/null | head -c128)
openssl enc -aes-256-ctr -pass pass:"$PASS" -nosalt </dev/zero \
| dd obs=$OBLK ibs=$IBLK of=$PARTITION \
oflag=direct status=progress
```
see [here](https://wiki.archlinux.org/title/Dm-crypt) for some light reading on the actual encryption part heh

### format
```
mkfs.fat -F 32 "$HD"1
fatlabel "$HD"1 BOOT
mkswap -L SWAP "$HD"2 
mkfs.ext4 -L ROOT "$HD"3
mkfs.ext4 -L HOME "$HD"4
```
