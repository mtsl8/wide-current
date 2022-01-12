### block device encryption

see [here](https://wiki.archlinux.org/title/Dm-crypt) and [here](https://wiki.artixlinux.org/Main/InstallationWithFullDiskEncryption) for some light reading on the actual encryption part heh

the short version is, you can encrypt the entire drive as one device, or choose specific partitions to encrypt. the way you choose to encrypt has many potential consequences on how you boot, and extra steps to be taken by the bootloader and kernel to do so. LUKS2 is the standard encryption header system. there are ways to backup the header and also to add multiple password keys or keyfiles, which can be stored mentally, locally, or on a detachable drive.
