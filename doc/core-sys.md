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

### [secure boot](secure-boot.md)

___

### [preparing hard drive(s) for installation](drives.md)

### [mounting hard drive(s) and installing the core system](install.md)
