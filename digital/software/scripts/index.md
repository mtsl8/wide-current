#### Required:

* artix installation [iso]() properly [installed]() on removable storage  
* a relatively modern (last decade ish) [x86_64]() PC with [UEFI]() & [secure boot](https://www.rodsbooks.com/efi-bootloaders/secureboot.html#whatis) disabled
  * note: secure boot [support]() coming soon
* an unused HDD. default settings use less than 250GB total. less is easily [do-able](/digital/software/conf/main.conf)...
  * __please note:__ *format* __scripts will erase selected hard drive;__ use carefully!
  * currently designed for HDD; SSD support coming soon
  * future versions will include instructions for:
    * using free partition space on a current (in-use) drive 
    * installing/booting alongside different OS's
    * SSDs with built in hardware encryption
* an internet connection via ethernet
  * wifi is not difficult to set up, but not currently supported. 
[(reason1)](/digital/emag-health.md) [(reason2)](/ditital/wireless/limitations.md)

* currently defaults to Nvidia proprietary drivers (which make a mess of things but support inexpensive GPUs)
* using intel or amd drivers is a relatively simple change; options provided in _main.conf_
___

#### 1. prepare installation:

download the latest release of this repository. 

edit the .conf files directly or use your own. directions in the comments

#### 2. prepare drive:

_format.sh_: creates new GPT partition table and uses the settings in<br/>
_format.conf_ to create, format and label the required partitions.

#### 3. mount and install base system

_bootstrap.sh_: connects to the internet, mounts newly created filesystem,<br/> 
(downloads/verifies/) installs system binaries, and chroots into the new installation.

#### 4. set up and populate new base system

_setup.sh_: 

sets up clock, localization, hosts & hostname, dhcpc and network manager,<br/>
root password, sudo, primary user and password.<br/>
installs core software and bootloader, then reboots.

#### 5. install {display, video, audio} {drivers, frameworks}

_
