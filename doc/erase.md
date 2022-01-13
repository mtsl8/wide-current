### erasing hard drives

The two main reasons to erase the drive are to delete the old data or to prepare for encryption. One could just write all 0's or 1's to the drive, but that will not bury the data well, nor is it a quality foundation for strong encryption - randomness is much preferred. To deeply/forensically erase, multiple passes may be required, depending on the specific hardware (above my paygrade). However, simply deleting the partition tables will render the data useless to the casual user: ```wipefs -af $DEVICE```. \[unverified: first use wipefs on each partition and then the drive to erase all of the "magic strings".\]

___

__NAND__ memory (flash, SSD, NVMe) does not enjoy being bit blasted; it reduces their lifespan. It can be done, and sometimes makes sense, but more often these days there will be some built-in functionality that is preferred. More sophisticated devices have built-in harware encryption made for the drive; buy [those](https://wiki.archlinux.org/title/Self-encrypting_drives) / use that when you can. It's not actually too pricey for top shelf NVMe, and will do the thing the fastest and easiest (essentially all you have to do is revoke the final key, rendering your data an unencryptable mess).

If you have a drive without that capability, there are a few options. To delete all block references in the SSD, without resetting the memory itself, use: ```blkdiscard $DEVICE```. In order to do a full factory reset, see [here](https://wiki.archlinux.org/title/Solid_state_drive/Memory_cell_clearing) (check nearby pages if that doesn't cover your drive). _There is a fair bit of complication in this department, due to the varying levels of hardware support._

___

__HDD__'s do not mind bit blasting at all, but it will take a while. /dev/urandom can be used directly to provide an unseeded random sequence, but it's faster to use _openssl_ with a urandom generated password. 

\[note: if you are using an older kernel (<5.6) you may benefit from using _rng-tools_ (hardware-based) or _haveged_ (software-based) to increase entropy. to see available entropy: ```cat /proc/sys/kernel/random/entropy_avail``` \]

replace "X" with the drive letter, and include a number "N" to target a specific partition. if the partition number is omitted the entire drive will be erased. many use _dd_, but _cat_ or _pv_ are naturally faster in most cases. _pv_ will display progress, _cat_ will not.

```
DEVICE="/dev/sdXN"
PASS=$(tr -cd '[:alnum:]' < /dev/urandom 2>/dev/null | head -c128)

openssl enc -aes-256-ctr -pass pass:"$PASS" -nosalt </dev/zero \
| pv > $DEVICE
```

___
___

If you want to take extra measures to isolate the contents of the drive you are erasing from a working (installed) system, here are two options:
* software: create a chroot jail (or some kind of virtual container) with the minimum toolset required to sanitize the drive
* hardware: use an installer ISO on a removable flash drive as a drive utility with any PC
  * for added security, unplug any other drives

#### chroot jail:

First, think about what is active and running on your system. if you login directly to a tty and then su, then chroot, then you are in the only authenticated interactive shell running while you access the device, and there is no way to pass commands out of the chroot back to the root user. at this point any malicious code would only have access to whatever tools you have installed there. In the worst case you could power down your computer without risk of privelage escalation, if you somehow lost the ability to exit the chroot. 

However, if the drive is not ever mounted, the filesystems are not connected or activated, and have no means by which to execute code in the running kernel. If you need to mount a drive you don't trust to recover data or run an executable file (application), that is where you start to open up more serious risks.[^1] (_The only class of viruses that would be effective from an unmounted drive[^2] would be firmware-based, which would require foreknowledge, physical presence, tools, and expertise to deploy.[^3]_)

from the initial login shell:
```
sudo su
JAIL="/mnt/chroot"
mkdir -p $JAIL
basestrap $JAIL util-linux pv
mount --bind $JAIL $JAIL
artix-chroot $JAIL

### connect drive
### erase drive

exit
umount $JAIL
# optionally remove (or shred) the chroot
rm -rf $JAIL
exit
```
___
___
[^1]: For further leverage, commands could be executed in shubshells by a minimally privelaged user, so they can be monitored and cancelled if they become corrupted. also, making sure to be disconnected from the internet vastly shrinks the attack surface. removing extra tty/getty's (login shells) could also limit the attack surface, in the event that some login credentials happened to be compromised.

[^2]: Assuming your current installation is clean, of course; a virus which was already in your operating system or on your PC's firmware could potentially find a way to access the files on the drive without your knowledge, or even exploit a known bug in the hard drive's firmware to do something. In that case though it wouldn't much matter, since the system is already corrupted - unless of course the bug was intentionally lying in wait.. again, above my paygrade.

[^3]: It would require a sophisticated virus embedded in the hard drive's BIOS to cause any problems; assuming the only commands to be run which interact with the drive are to query the bus (lsblk) and to write random meaningless data (openssl|pv>drive), neither of which request to read or execute data from the filesystems on the drive. Likewise, SSD related commands for erasure are directed at the _operating system_ of the hard drive, not the _filesystems_ it contains. There is no normal means by which information could jump from the filesystem to the ROM; it would have to be manually, physically installed using a ROM flashing device, inside the case of the drive, reverse engineered to the specific make and model, and any software mistakes would likely render the drive nonfunctional (assuming there is enough extra room on the chip to do something nefarious with).
