### mount
```
swapon "$HD"2
mount "$HD"3 /mnt
mkdir -p /mnt/{efi,home}
mount "$HD"1 /mnt/efi
mount "$HD"4 /mnt/home
```

any extra partitions can be mounted based on the location and type of user permissions you want them to have.

* the root user owns "/\*" including "/home/" 
* each user owns their "/home/$user/" directory and everything it contains by default

some info on user mounts and FSTAB options:<br>
https://superuser.com/questions/320415/mount-device-with-specific-user-rights

(note that user-id mounts via FSTAB are not available for ext4, so a better solution may be to add a runit start service with the correct mount command)

some notes on security via mount options [here](https://wiki.archlinux.org/title/Security#Mount_options)

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
