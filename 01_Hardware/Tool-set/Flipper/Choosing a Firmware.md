
Personal preverence is currently The [Mommentum Firmware](Mommentum.md). 
## Flashing a firmware that you compiled yourself 

The following is only relevant if you want to use the latest build and not just a release version. 

this flashes directly to the flipper. qFlipper needs to be closed and the flipper connected via cable to the pc. 
```shell
./fbt flash_usb_full
```

the following command creates a package that can the be flashed using the qFlipper app. 
```shell
./fbt updater_package
```