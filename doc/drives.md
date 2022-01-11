### [partitioning the drive(s)

the default path here assumes you are using a free (== unused and prepared to be ERASED) HD; </br>

_information about dual booting with another os depends on how they boot. dual booting with Windows is possible and less complicated than it might look [here](https://wiki.archlinux.org/title/Dual_boot_with_Windows). grub can manage most modern systems. more to come on that subject in the future..._

as far as how much to allocate, that depends on what you want to do with the system. <br>

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

example partitions:
_(replace "X" with the letter of the drive you are installing to:)_
```
artix
artix

su

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