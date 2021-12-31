* blink:
  * [brave-bin](https://gitea.artixlinux.org/Universe/brave/src/branch/main/PKGBUILD)^
  * [ungoogled-chromium](https://github.com/Eloston/ungoogled-chromium)*^
* gecko:
  * [firefox-esr](https://gitea.artixlinux.org/Universe/firefox-esr)
  * [librewolf](https://librewolf.net/)* (privacy-oriented fork of firefox)
    * developer recommended addons: [@librewolf.net](https://librewolf.net/docs/addons/)
    * maintainer & community suggestions : [@forum.artixlinux.net](https://forum.artixlinux.org/index.php/topic,1687.0.html)
  * [firedragon](https://github.com/dr460nf1r3/firedragon-browser)  (fork of librewolf)
  * [tor-browser](https://tb-manual.torproject.org/)
* [thunderbird-artix](https://gitea.artixlinux.org/Universe/thunderbird-artix/src/branch/main/PKGBUILD)* (mail client w/ PGP encryption support)

all available in the artix [universe repository](https://gitea.artixlinux.org/Universe/) :

_(note: firedragon only appears on mirror, no PKGBUILD listed; see [here](https://aur.archlinux.org/packages/firedragon/) for likely build instructions)_

/etc/pacman.conf:

```
[universe]
Server = https://universe.artixlinux.org/$arch
```
___

\* included in default web.conf

^ (blink-any??) required for obs-studio browser integration
