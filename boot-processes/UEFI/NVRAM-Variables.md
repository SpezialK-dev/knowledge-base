
This is mainly research,so take everything with a grain of salt here. 

# Inital Look into things 

- https://github.com/myspaghetti/virtualbox-generate-nvram-bin-file
- https://github.com/infokiller/win10-vm?tab=readme-ov-file#using-uefi-firmware-with-the-required-keys
- https://deepwiki.com/LongSoft/UEFITool/3.1-uefitool-gui#overview
- https://ally-petitt.com/en/posts/2024-07-05_emulating-with-nvram/
- https://github.com/LongSoft/UEFITool/blob/new_engine/common/nvramparser.cpp
- https://ruuucker.github.io/EFI-Basics-NVRAM-Variables/
## Using QEMU

on systems that use a 


the path for your variables of the different VM's  is 

```
/var/lib/libvirt/qemu/nvram/*.fd
```

it appears that there is a difference between after booting, a linux, so if you are booting something with a shim it appears to create an entry into the nvram variables. 


```
00009440: 7600 6500 6c00 0000 7362 6174 2c31 2c32  v.e.l...sbat,1,2
00009450: 3032 3130 3330 3231 380a ffff aa55 3f00  021030218....U?.
00009460: 0700 0000 0000 0000 0000 0000 0000 0000  ................
00009470: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00009480: 1200 0000 8000 0000 61df e48b ca93 d211  ........a.......
00009490: aa0d 00e0 9803 2b8c 4200 6f00 6f00 7400  ......+.B.o.o.t.
000094a0: 3000 3000 3000 3500 0000 0100 0000 6c00  0.0.0.5.......l.
000094b0: 4600 6500 6400 6f00 7200 6100 0000 0201  F.e.d.o.r.a.....
000094c0: 0c00 d041 030a 0000 0000 0101 0600 021f  ...A............
000094d0: 0312 0a00 0100 ffff 0000 0402 1800 0100  ................
000094e0: 0000 675a 1300 0000 0000 00f0 0000 0000  ..gZ............
000094f0: 0000 0404 3400 5c00 4500 4600 4900 5c00  ....4.\.E.F.I.\.
00009500: 6600 6500 6400 6f00 7200 6100 5c00 7300  f.e.d.o.r.a.\.s.
00009510: 6800 6900 6d00 7800 3600 3400 2e00 6500  h.i.m.x.6.4...e.
00009520: 6600 6900 0000 7fff 0400 ffff aa55 3f00  f.i..........U?.
00009530: 0700 0000 0000 0000 0000 0000 0000 0000  ................
00009540: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00009550: 1400 0000 0c00 0000 61df e48b ca93 d211  ........a.......
00009560: aa0d 00e0 9803 2b8c 4200 6f00 6f00 7400  ......+.B.o.o.t.
00009570: 4f00 7200 6400 6500 7200 0000 0500 0200  O.r.d.e.r.
```

an example, to replicate this, you add a SATA CDROM again this time with a fedora ( in this case) when this boots normally this entry gets written. Just selecting adding the CDROM back and selecting the new media there does not add anything. But once its booted you get a MOK screen as well as this new entry. 


now after rebooting into windows this stays persistent, and some more data has been written into the the NVRAM. Even though I only temporarily booted a Linux and did not change anything in the actual boot order. 


# Accessing NVRAM variables from userspace

linux should map them into `firmware/efi/efivars`, you could write your own lib to paarse them from the filesystem but that is not needed
since [efivars](https://github.com/rhboot/efivar)([Archlinux package](https://archlinux.org/packages/core/x86_64/efivar/)) exist. With this you can simply access them. 

To iterate through simply use `int efi_get_next_variable_name(efi_guid_t **guid**, char **name)`
in combination with a loop, the func returns 0 when the iteration is complete so its quite easy to build something around that.

# Found Tooling 


- https://github.com/LongSoft/UEFITool
- https://github.com/tianocore/edk2/tree/master/BaseTools/Source/Python/FMMT
- https://github.com/linuxboot/fiano
- https://github.com/theopolis/uefi-firmware-parser
- https://github.com/chipsec/chipsec
- https://www.kernel.org/doc/html/latest/filesystems/efivarfs.html
- https://github.com/datasone/setup_var.efi
- https://github.com/GeographicCone/UefiVarTool