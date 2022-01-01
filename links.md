[Artix](https://artixlinux.org/) - _The Art of Linux_ - Simple. Fast. Systemd-free

( ... open-init, developer-conscious fork of: ... )

[Arch](https://archlinux.org/) - A simple, lightweight distribution

( ... rolling release, binary distribution of: ... )

[Linux](https://www.kernel.org/) - a clone of Unix, written from scratch

( ... runtime operating systems, I/O, drivers and device firmware \[canonically\] compiled and utilized by: ... )

[GNU](https://www.gnu.org/software/software.html) - a powerful collection of open-source tools, including:

* grub
* coreutils
* bash
* grep
* nano
* gnutls
* binutils
* bison
* glibc
* gcc
* make
* autoconf/automake

___

systemd-free distributions on the lighter side from Artix:

[glibc](https://www.gnu.org/software/libc/) based: 
* [Gentoo](https://www.gentoo.org/) - source & portage
* [Devuan](https://www.devuan.org/) - Debian based
* [antiX](https://antixlinux.com/) - same, but perhaps half the size
* [LFS](https://linuxfromscratch.org/lfs/view/stable/) - build GNU/Linux completely from source, with detailed instructions
* [CLFS](http://clfs.org/view/CLFS-3.0.0-SYSVINIT/x86_64-64/) - simpler system than LFS (stale ~ 2014)

[musl](https://git.musl-libc.org/cgit/musl/) based:
* [Adélie](https://git.adelielinux.org/adelie?sort=latest_activity_desc)
* [Alpine](https://alpinelinux.org/)
* [sabotage](https://github.com/sabotage-linux/sabotage)
* [mkroot](https://github.com/landley/mkroot)

~ ~ ~

VHDL: [J-core](https://j-core.org/)<br/>
FPGA: [turtle-board](https://www.cnx-software.com/2017/03/13/turtle-board-is-a-raspberry-pi-2-like-fpga-board-for-j-core-j2-open-source-superh-sh2-soc/) - _project currently [unfun](/img/sadge-king_cookk.png)ded (afaik)_

__based__

___

### Tools

aka ~ util-linux
* [toybox](https://github.com/landley/toybox)
* [busybox](https://www.busybox.net/)
* [cygwin](https://www.cygwin.com/)

shell
* [dash](http://gondor.apana.org.au/~herbert/dash/) 82.6 KB / 149.8 KB
* [bash](https://tiswww.case.edu/php/chet/bash/bashtop.html) 1.6 MB / 8.2 MB
* [zsh](https://www.zsh.org/) 2.1 MB / 6.2 MB

transfer over network
* [rsync](https://rsync.samba.org/) 308.3 KB / 579.8 KB
* [curl](https://curl.se/) 1.1 MB / 1.8 MB
* [wget](https://www.gnu.org/software/wget/) 739.1 KB / 3.1 MB
* [hg](https://www.mercurial-scm.org/) 4.9 MB / 26.0 MB
* [git](https://git-scm.com/) 5.9 MB / 32.9 MB

ssh
* [dropbear](https://matt.ucc.asn.au/dropbear/dropbear.html) 
  * 233.2 KB pkg / 1.0 MB installed; deps: libxcrypt, zlib, git(make)
* [openssh](https://www.openssh.com/) 
  * 1011.4 KB pkg / 5.9 MB installed; runtime deps: glibc, krb5, ldns, libxcrypt, libedit, openssl, pam, zlib 

package management

* [re:clean-chroot](https://wiki.archlinux.org/title/DeveloperWiki:Building_in_a_clean_chroot)
* [artools](https://gitea.artixlinux.org/artix/artools)
* [makepkg](https://wiki.archlinux.org/title/Arch_Build_System)
* [pacman](https://gitlab.archlinux.org/pacman/pacman/)

toolchain
* [clang](https://clang.llvm.org/) / [llvm](https://llvm.org/)
* [gcc](https://www.gnu.org/software/gcc/) / [binutils](https://www.gnu.org/software/binutils/)
* [musl-cross-make](https://github.com/richfelker/musl-cross-make) / [sabotage-headers](https://github.com/sabotage-linux/kernel-headers)
* [ninja](https://ninja-build.org/) / [meson](https://mesonbuild.com/) / [cmake](https://cmake.org/)
* ~ ~ ~
* [openjdk](https://openjdk.java.net/) / [Node.js](https://nodejs.org/en/) / npm
* [python](https://www.python.org/) / pip
* [perl](https://www.perl.org/)
* [lua](https://www.lua.org/)
* ~ ~ ~
* [Rust](https://www.rust-lang.org/) / cargo
* [C++](https://isocpp.org/)
* [__C__](http://www.open-std.org/jtc1/sc22/wg14/)
* assembly, interrupts, CMOS, TTL, PWM, [Quartz](https://www.avrfreaks.net/sites/default/files/introductionquartz.pdf), metal, current
* [transistor](https://en.wikipedia.org/wiki/Transistor)
* [lisp](https://lisp-lang.org/)
* [lamda-calculus](https://plato.stanford.edu/entries/lambda-calculus/)

init

* [sysvinit](https://wiki.gentoo.org/wiki/Sysvinit)
* [OpenRC](https://wiki.gentoo.org/wiki/OpenRC)
* [runit](http://smarden.org/runit/)
* [s6](https://www.skarnet.org/software/s6-linux-init/)
* [suite66](https://web.obarun.org/software/66/latest/66-init.html)
* ~ ~ ~
* [mdev](https://github.com/slashbeast/mdev-like-a-boss)
* [eudev](https://wiki.gentoo.org/wiki/Eudev)
* [elogind](https://wiki.gentoo.org/wiki/Elogind)

boot
* [iPXE](https://ipxe.org/)
* [coreboot](https://coreboot.org/)
* [Heads:ROM](https://osresearch.net/)
* [rEFInd](http://www.rodsbooks.com/refind/)
* [grub](https://www.gnu.org/software/grub/)
* [mkinitcpio](https://wiki.archlinux.org/title/Mkinitcpio)
* [mokutil](https://github.com/lcp/mokutil)
* [shim-signed](https://aur.archlinux.org/packages/shim-signed/)
* [re:secure-boot](https://www.rodsbooks.com/efi-bootloaders/secureboot.html)
