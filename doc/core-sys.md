## core system

think of this as a cheat-sheet for installation so you don't have to wiki all over the place

take and adjust based on your particular needs. do read wikis to understand things.

do not just blindly copy and paste. 

(installer scripts will be released when they are tested and documented)

### overview

* host = [artix](https://artixlinux.org/)
* package manager = [pacman](https://wiki.archlinux.org/title/Pacman) + [artools](https://gitea.artixlinux.org/artix/artools)
* mirror = artix [(gitea)](https://gitea.artixlinux.org/artixlinux)
  * todo: [local cache server](https://xyne.dev/projects/pacserve/) (reduce load on artix mirror)
* kernel = [linux-zen](https://github.com/zen-kernel/zen-kernel)
  * scheduler: bfq, high resolution, highly responsive, congestion control
  * more details at https://liquorix.net/ (binary distribution)
* init = [runit](http://smarden.org/runit/)
* bootloader = [grub](https://www.gnu.org/software/grub/)
___

### [acquire hardware](hardware.md)

### [iso > media](install-media.md)
\[note: gpg is provided by the package "gnupg"\]

### [re: secure boot](secure-boot.md)

### boot host system

* if on modern EFI (current standard), in the boot setup menu:
<br>__ to check boot mode see [here](https://github.com/mtsl8/wide-current/blob/main/doc/drives.md#determining-boot-mode) __<br>
  * disable secure boot
  * disable legacy/CSM emulation mode (adds boot time and potential confusion, if BIOS booting is not desired/required)
* to choose the installation media as a boot source:
  * set the boot order in the boot setup/menu, so that your media is read before the hard drive;
  * or use the boot options key at startup to select it from a list of available devices
* in the host boot menu, select the type of media you are booting from (optical vs. flash/HD)
* there may be some keyring stuff whizzing by; 
* when it's finished, press a key on the keyboard to activate the getty
* you will arrive at a login shell; user:artix pass:artix
* type "su" to become root and begin.

___

### [preparing hard drive(s) for installation](drives.md)

### [mounting hard drive(s) and installing the core system](install.md)
