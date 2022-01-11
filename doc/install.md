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
dhcpcd && ping -w 5 -c 10 widemage.net
```
### install
```
basestrap /mnt base base-devel runit elogind-runit 
basestrap /mnt linux-zen linux-firmware {intel|amd}-ucode
```
* [root-install](/src/root-install.packages)
* [wm-install](/src/wm-install.packages)
