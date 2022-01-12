### erasing hard drives

The two main reasons one would want to erase the drive are to delete the old data or to prepare for encryption. One could just write all 0's or 1's to the drive, but that will not bury the data well, nor is it a quality foundation for strong encryption - randomness is much preferred. To deeply erase, multiple passes may be required; however, simply deleting the partition table will render most data useless to the casual user.

NAND memory (flash, SSD, NVMe) does not enjoy being bit blasted; it reduces their lifespan. More sophisticated devices have built in harware encryption made for the drive; buy those / use that! It's not actually too pricey for primo NVMe. In order to factory reset an SSD which doesn't have those capabilities, see [here](https://wiki.archlinux.org/title/Solid_state_drive/Memory_cell_clearing) (check nearby pages if that doesn't cover your drive)

HDD's do not mind bit-blasting at all, but it will take a while. /dev/urandom can be used directly to provide a unseeded random sequence, but it's a lot faster to use _openssl_ with a urandom generated password. Alternatively, if the system is low on entropy and you want to use urandom directly, generation speed can be improved using or _rng-tools_ (hardware based) or _haveged_ (software based) \[see archwiki.org\].

replace "X" with the drive letter, and include a number "N" to target a specific partition. if the "N" is omitted the entire drive will be erased.

many will use _dd_, but _cat_ or _pv_ are naturally faster in most cases. _pv_ will display progress, _cat_ will not.

```
DEVICE="/dev/sdXN"

PASS=$(tr -cd '[:alnum:]' < /dev/urandom 2>/dev/null | head -c128)
openssl enc -aes-256-ctr -pass pass:"$PASS" -nosalt </dev/zero \
| pv > $DEVICE

```
