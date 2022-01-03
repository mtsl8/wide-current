## wide-current

scripts to install a simple but complete (and secure) [artix](https://artixlinux.org/) [linux](https://www.kernel.org/) system onto a persistent storage device

note: this is a work in progress. __(this blob will be removed when basic version is tested and working)__. new features are being added as time and resources allow.</br>

current iteration is a set of plain text configuration files (yml) with matching install scripts (sh); in the future more options will be supported, and prettier interfaces will be made.</br>

meanwhile here is a simple way to configure installations flexibly without having to repeat your logic again later.</br>

___

all of the scripts (will) reference the same core set of configuration files, and make sure they come with you through the installation process. this way, if you decide to change something later, the system can check its state and know what needs to be adjusted to make it work, and how. this state then also serves as the minimal template capable of rebuilding your system from scratch (not to be confused with resurrecting lost user data).

\[package managers can resolve dependencies, but they don't understand the subtleties of system design. backups are also a good idea, but not nearly as light/portable.\]

the core system is designed to be modern and fully functional while minimizing bloat and cruft, thereby gaining security and performance ~  software leverage on existing hardware. it is also designed to be easy to update, and to stay aligned with upstream whenever possible.

there (will be) are scripts to assist in downloading and installing office and utility software, web browsers and email client, obs-studio (with streamelements/browser integration), everything needed to launch fabric modded minecraft, and other open source tools for image, video, and audio processing. 

there is no requirement to use scripts you do not need; the latter ones are primarily just wrappers on makepkg and pacman which gather software that is known to work well together. wherever there are known complications, the scripts have "best" known solutions embedded. they also provide a common interface for logic to keep the ideal order of operations intact.

(an attempt at making a complicated digital ecosystem easier for humans to manage, without taking away freedom)

see the [docs](/doc/index.md) for more info.

___

I will not personally support features I don't use, for the simple reason that I can't verify their function ... but I will do my best to make the interfaces transparent and simple to extend. feel free to fork and adjust accordingly.

[recommended reading](/links.md) </br>
this is the deep end, jump in at your own responsibility.</br>
that said, I will do my best to help if you [contact](/contact.md) me with clear questions and/or clear data. </br>
that said, I also take no responsibility or liability for what you do with your computer. see [license](/LICENSE.md)
