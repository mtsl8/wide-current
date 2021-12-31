### Overview

* host = [artix](https://artixlinux.org/)
* package manager = [pacman](https://wiki.archlinux.org/title/Pacman) + [artools](https://gitea.artixlinux.org/artix/artools)
* mirror = artix [(gitea)](https://gitea.artixlinux.org/artixlinux)
  * todo: [local cache server](https://xyne.dev/projects/pacserve/)
* kernel = [linux-zen](https://github.com/zen-kernel/zen-kernel)
  * scheduler: bfq, high resolution, highly responsive, congestion control
  * more details at https://liquorix.net/ (binary distribution)
* init = [runit](http://smarden.org/runit/)
* bootloader = [grub](https://www.gnu.org/software/grub/)

### Packages

* __basestrap__
  * linux-zen linux-firmware {intel-ucode|amd-ucode}
  * base runit elogind-runit 
  * {nano|vim} sudo curl git
* __bootloader__
  * grub efibootmgr 

### [Scripts](/scripts/index.md)
