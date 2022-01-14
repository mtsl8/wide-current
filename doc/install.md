### mount

(if you have restarted since formatting the partitions, make sure to reset the HD variable or this won't work)

```
swapon "$HD"2
mount "$HD"3 /mnt

# if EFI:
mkdir /mnt/efi
mount "$HD"1 /mnt/efi
# elif BIOS:
mkdir /mnt/boot
mount "$HD"1 /mnt/boot
# fi

mkdir /mnt/home
mount "$HD"4 /mnt/home
```

[mounting additional or user partitions](mount-extra.md)

### connect
```
dhcpcd && ping -w 5 -c 10 widemage.net
```
### install

select the microcode package that matches your processor (intel-ucode | amd-ucode)

```
basestrap /mnt base base-devel runit elogind-runit 
basestrap /mnt linux-zen linux-firmware {intel|amd}-ucode
```
* [root-install](/src/root-install.packages)
* [wm-install](/src/wm-install.packages)

### tips

* keyboard shortcuts
* general context
