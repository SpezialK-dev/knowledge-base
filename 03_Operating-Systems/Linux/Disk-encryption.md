
# How to unlock a LUKS partion from a live Arch boot

this if you need to rescue a installation from a live boot
```shell 
# cryptsetup open /dev/<path_todevice> root #unlocks the device
# mount /dev/mapper/root /mnt # mounts the encrypted device
# arch-chroot /mnt  # actually logs into the shell there
```