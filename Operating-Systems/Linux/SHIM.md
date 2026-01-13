
shim allows you to secure boot using the Microsoft signed keys.

This can be useful when dual booting 

# disabling checking
if you want to load unsigned kernel modules or even an unsigned kernel but still have secure boot enabled in the bios you can simply disable shim checking for those things with the user land application mokutil

```shell
mokutil --disable-validation
```

this disables verification, and this allows unsigned code to run after the shim.
while still allowing you to secure boot. This also persists between installations. since it is stored as a uefi variable most likely on the motherboard. 

even if you have set a password with 


## password
```shell
sudo mokutil --password
```
this does not get asked anywhere only the password you give for disable validation so the password you enter is ignored.  


You can also simply clear the password anyway. 
```shell
sudo mokutil --clear-password
```

simply clears it only asking for the root password of your user. 
