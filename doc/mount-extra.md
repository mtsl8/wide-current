
any additional partitions can be mounted based on the location and type of user permissions you want them to have.

* the root user owns "/\*" including "/home/" 
* each user owns their "/home/$user/" directory and everything it contains by default

some info on user mounts and FSTAB options:<br>
https://superuser.com/questions/320415/mount-device-with-specific-user-rights

(note that user-id mounts via FSTAB are not available for ext4, so a better solution may be to add a _runit_ "start" service with the correct _mount_ command)

some notes on security via mount options [here](https://wiki.archlinux.org/title/Security#Mount_options)

any additions or modifications can be made to the FSTAB after using "fstabgen" to create it during installation.
