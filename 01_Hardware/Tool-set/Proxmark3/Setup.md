
# A small guid on how to set up your proxmark under Arch linux

[Installation Guid](https://github.com/RfidResearchGroup/proxmark3/blob/master/doc/md/Installation_Instructions/Linux-Installation-Instructions.md)
[Compilation instructions](https://github.com/RfidResearchGroup/proxmark3/blob/master/doc/md/Use_of_Proxmark/0_Compilation-Instructions.md)

## Choosing a firmware 

The normal firmware proxmark firmware seems to be outdated ? The most maintained firmware seems to be the one from [iceman](https://github.com/RfidResearchGroup/proxmark3) . Here we also have documentation for Arch linux.


## Installing the right dependencies 

```shell
sudo pacman -Syu git base-devel readline bzip2 lz4 arm-none-eabi-gcc \
arm-none-eabi-newlib qt5-base bluez python gd --needed
```

IMPORTANT remove [modem manager ](https://github.com/RfidResearchGroup/proxmark3/blob/master/doc/md/Installation_Instructions/ModemManager-Must-Be-Discarded.md)


the clone the repo
```shell
git clone https://github.com/RfidResearchGroup/proxmark3.git
```

# Issues

## Compilation

```shell
/bin/sh: line 1: ./bmptst: Permission denied
make[2]: *** [Makefile:34: bmptst] Error 1
make[1]: *** [Makefile:949: deps/reveng/libreveng.a] Error 2
make[1]: *** Waiting for unfinished jobs....
In file included from /usr/include/qt/QtCore/qfuture.h:45,
                 from /usr/include/qt/QtCore/QtCore:97,
                 from /usr/include/qt/QtGui/QtGuiDepends:3,
                 from /usr/include/qt/QtGui/QtGui:3,
                 from src/proxguiqt.h:30,
                 from src/proxguiqt.cpp:19:
/usr/include/qt/QtCore/qfutureinterface.h:284:37: error: template-id not allowed for constructor in C++20 [-Werror=template-id-cdtor]
  284 |     explicit QFutureInterface<void>(State initialState = NoState)
      |                                     ^~~~~
/usr/include/qt/QtCore/qfutureinterface.h:284:37: note: remove the ‘< >’
In file included from /usr/include/qt/QtCore/qfuture.h:45,
                 from /usr/include/qt/QtCore/QtCore:97,
                 from /usr/include/qt/QtGui/QtGuiDepends:3,
                 from /usr/include/qt/QtGui/QtGui:3,
                 from src/proxguiqt.h:30,
                 from src/proxgui.cpp:22:
/usr/include/qt/QtCore/qfutureinterface.h:284:37: error: template-id not allowed for constructor in C++20 [-Werror=template-id-cdtor]
  284 |     explicit QFutureInterface<void>(State initialState = NoState)
      |                                     ^~~~~
/usr/include/qt/QtCore/qfutureinterface.h:284:37: note: remove the ‘< >’
In file included from /usr/include/qt/QtCore/qfuture.h:45,
                 from /usr/include/qt/QtCore/QtCore:97,
                 from /usr/include/qt/QtGui/QtGuiDepends:3,
                 from /usr/include/qt/QtGui/QtGui:3,
                 from src/proxguiqt.h:30,
                 from src/proxguiqt.moc.cpp:10:
/usr/include/qt/QtCore/qfutureinterface.h:284:37: error: template-id not allowed for constructor in C++20 [-Werror=template-id-cdtor]
  284 |     explicit QFutureInterface<void>(State initialState = NoState)
      |                                     ^~~~~
/usr/include/qt/QtCore/qfutureinterface.h:284:37: note: remove the ‘< >’

```

was the error that I encountered

The trouble shooting steps that I took to fix this issue.

fixing the permission problems was a problem on my side, where my drive permissions where configured incorrectly after fixing that we are still left with the error in compiling the software. 

I think the easiest quickest solution would be to disable QT gui features since a terminal interface would be enough for me. 

### running it without QT enabled 

since QT refuses to work I will need to do a small work around, I dont want to thinker around in the header files since that could and most likely would break my system.


Now I am trying to build it with the Docker image for archlinux, since these are firstly isolated and secondly they dont have the QT problem (since they dont use QT)

This seemed to have worked?
It passed through so it compiled something???

If there is a way to disable working with qt I have not found it. 
##  Flashing


```shell
[+] Waiting for Proxmark3 to appear on /dev/ttyACM0
 🕑  59 found
[+] Entering bootloader...
[+] (Press and release the button only to abort)
[+] Trigger restart...
[+] Waiting for Proxmark3 to appear on /dev/ttyACM0
 🕐  50 found
[!!] 🚨 ====================== OBS ! ===========================================
[!!] 🚨 Note: Your bootloader does not understand the new CMD_BL_VERSION command
[!!] 🚨 It is recommended that you first update your bootloader alone,
[!!] 🚨 reboot the Proxmark3 then only update the main firmware


[!!] 🚨 ------------- Follow these steps -------------------

[!!] 🚨  1)   ./pm3-flash-bootrom
[!!] 🚨  2)   ./pm3-flash-fullimage
[!!] 🚨  3)   ./pm3

[=] ---------------------------------------------------

[=] Available memory on this board: UNKNOWN

[!!] 🚨 ====================== OBS ! ======================================
[!!] 🚨 Note: Your bootloader does not understand the new CHIP_INFO command
[=] Permitted flash range: 0x00100000-0x00140000
[+] Loading usable ELF segments:
[+]    0: V 0x00100000 P 0x00100000 (0x00000200->0x00000200) [R X] @0x94
[+]    1: V 0x00200000 P 0x00100200 (0x000014b0->0x000014b0) [R X] @0x298

[+] Loading usable ELF segments:
[+]    0: V 0x00102000 P 0x00102000 (0x00056424->0x00056424) [R X] @0x98
[!!] 🚨 Error: PHDR is not contained in Flash
[!!] 🚨 Firmware is probably too big for your device
[!!] 🚨 See README.md for information on compiling for platforms with 256KB of flash memory
[!] ⚠️  The flashing procedure failed, follow the suggested steps!
[+] All done

```

after trying that did it flash?

```shell
./pm3-flash-bootrom 
[=] Session log /home/spezialk/.proxmark3/logs/log_20240924210906.txt
[+] About to use the following file:
[+]    <path>/proxmark3/client/../bootrom/obj/bootrom.elf
[+] Loading ELF file <path>/proxmark3/client/../bootrom/obj/bootrom.elf
[+] ELF file version Iceman/master/v4.18994-127-gd2783214e-suspect 2024-09-24 16:50:44 7bbf866a9

[+] Waiting for Proxmark3 to appear on /dev/ttyACM0
 🕑  59 found
[+] Entering bootloader...
[+] (Press and release the button only to abort)
[+] Trigger restart...
[+] Waiting for Proxmark3 to appear on /dev/ttyACM0
 🕐  50 found
[!!] 🚨 ====================== OBS ! ===========================================
[!!] 🚨 Note: Your bootloader does not understand the new CMD_BL_VERSION command
[!!] 🚨 It is recommended that you first update your bootloader alone,
[!!] 🚨 reboot the Proxmark3 then only update the main firmware


[!!] 🚨 ------------- Follow these steps -------------------

[!!] 🚨  1)   ./pm3-flash-bootrom
[!!] 🚨  2)   ./pm3-flash-fullimage
[!!] 🚨  3)   ./pm3

[=] ---------------------------------------------------

[=] Available memory on this board: UNKNOWN

[!!] 🚨 ====================== OBS ! ======================================
[!!] 🚨 Note: Your bootloader does not understand the new CHIP_INFO command
[=] Permitted flash range: 0x00100000-0x00140000
[+] Loading usable ELF segments:
[+]    0: V 0x00100000 P 0x00100000 (0x00000200->0x00000200) [R X] @0x94
[+]    1: V 0x00200000 P 0x00100200 (0x000014b0->0x000014b0) [R X] @0x298

[+] Flashing...
[+] Writing segments for file: <user_path>/proxmark3/client/../bootrom/obj/bootrom.elf
[+]  0x00100000..0x001001ff [0x200 / 1 blocks]
. ok
[+]  0x00100200..0x001016af [0x14b0 / 11 blocks]
........... ok

[+] All done

[=] Have a nice day!

```

it appears to have flashed correclty 
after executing to also flash the firmware
```shell
 ./pm3-flash-fullimage
```

it now finally works!! 
