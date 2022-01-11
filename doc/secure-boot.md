currently secure boot is not supported;<br>
_future support using fedora's_ [shim-signed](https://aur.archlinux.org/packages/shim-signed/) _and_ [mokutil](https://github.com/lcp/mokutil) _planned_<br>

secure boot is of questionable security itself because it is not open source, so we don't actually know what it's doing.
the idea is good though, I think.. best practices are good habits to be in. "single root of trust"

for linux however it is mostly useful as protection from in-person tampering.<br>
see: [here](https://en.wikipedia.org/wiki/Linux_malware), and search for anything related to boot sector viruses...

encryption and proper system management are in general more useful for security than secure boot.

\[note: unless you are using something like Bitlocker and storing the key in the TPM, 
disabling secure boot should not interfere with booting another OS, and re-enabling will restore TPM functionality\]
