## core-sys

think of this as a cheat-sheet for installation so you don't have to wiki all over the place

take and adjust based on your particular needs. do read wikis to understand things.

do not just blindly copy and paste.. it is not quite that refined yet.

(when it is it will be named .sh)

### overview

* host = [artix](https://artixlinux.org/)
* package manager = [pacman](https://wiki.archlinux.org/title/Pacman) + [artools](https://gitea.artixlinux.org/artix/artools)
* mirror = artix [(gitea)](https://gitea.artixlinux.org/artixlinux)
  * todo: [local cache server](https://xyne.dev/projects/pacserve/)
* kernel = [linux-zen](https://github.com/zen-kernel/zen-kernel)
  * scheduler: bfq, high resolution, highly responsive, congestion control
  * more details at https://liquorix.net/ (binary distribution)
* init = [runit](http://smarden.org/runit/)
* bootloader = [grub](https://www.gnu.org/software/grub/)
___

### acquire hardware

### iso > media

### secure boot

currently secure boot is not supported;<br>
_future support using fedora's_ [shim-signed](https://aur.archlinux.org/packages/shim-signed/) _and_ [mokutil](https://github.com/lcp/mokutil) _planned_<br>

secure boot is of questionable security itself because it is not open source, so we don't actually know what it's doing.
the idea is good though, I think.. best practices are good habits to be in. "single root of trust"

for linux however it is mostly useful as protection from in-person tampering.<br>
see: [here](https://en.wikipedia.org/wiki/Linux_malware), and search for anything related to boot sector viruses...

encryption and proper system management are in general more useful for security than secure boot.

\[note: unless you are using something like Bitlocker and storing the key in the TPM, 
disabling secure boot should not interfere with booting another OS, and re-enabling will restore TPM functionality\]

### partition

the default path here assumes you are using a free (== unused and prepared to be ERASED) HD; </br>

_information about dual booting with another os depends on how they boot. dual booting with Windows is possible and less complicated than it might look [here](https://wiki.archlinux.org/title/Dual_boot_with_Windows). grub can manage most modern systems. more to come on that subject in the future..._

as far as how much to allocate, that depends on what you want to do with the system. <br>
using a small efi boot partition is possible as long as you store your kernels on the root partition, which is wide-default. <br>
the root partition can be smaller than 40 if the system is light. I usually use 100 if the drive is big. <br>
home at minimum needs to be at least enough to store configuration and working files, so not that big really <br>
one could use a seperate partition/drive for audio and yet another larger one for video.. for example. <br>
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
# ROOT: 40G linux filesystem
# HOME: 100G linux filesystem
```
### erase, encrypt ~ incomplete, optional

Encryption on all ones zeroes is not very strong. encryption on random is.<br>
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
### mount
```
swapon "$HD"2
mount "$HD"3 /mnt
mkdir -p /mnt/{efi,home}
mount "$HD"1 /mnt/efi
mount "$HD"4 /mnt/home
```
### connect
```
dhcpcd && ping widemage.net
```
### install
```
basestrap /mnt base base-devel runit elogind-runit 
basestrap /mnt linux-zen linux-firmware {intel|amd}-ucode
```
* [root-install](/src/root-install.packages)
* [wm-install](/src/wm-install.packages)
