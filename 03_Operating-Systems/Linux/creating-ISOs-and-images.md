# Debian 

## packages required under arch linux 

- libisoburn (xorriso)
- rsync
- squashfs-tools
## Some guids 

This is a guid on creating a Debian image with some modified InitramFS. To allow
[A small guid (0)](https://d-i.debian.org/doc/internals/ch04.html)
A better guid that isnt offical seems this [Linuxquestions (1)](https://www.linuxquestions.org/questions/debian-26/tutorial-creating-a-custom-bootable-debian-live-iso-4175705804/)
with some adjustments from the [debian wiki: repack bootable iso (2)](https://wiki.debian.org/RepackBootableISO). 

## With secure boot

since we want to maintain the securebootability of this we will use a already signed instllation for this we will get a normal Installation of your choosing for debian.

the following commands are taken from Guid 1.

firstly we are geting out a isohdpfx.bin from the image 
```shell
dd if=<your debain ISO here>.iso bs=1 count=432 of=isohdpfx.bin
```

this creates the proper dirs in our current directory 
```shell
mkdir isoextract mnt
```

>OPTIONAL : 
> elevate to root with su or use the sudo in-front of these commands your choice

 
first we will mount it as a loopback device to mnt 
```shell
sudo mount -o loop <your debain ISO here>.iso mnt
```
then we will get all of the data from rsing excluding the squashfs system and copy t hat to the iso extracted dir 
```shell
rsync --exclude=/live/filesystem.squashfs -a mnt/ isoextract
```

Some steps are missing from here since the squasfs filesystem would not show up So I am trying a different aproach. with the Arch image. 
# Creating a secured boot Arch image


## Required packages to follow this guid 

- [ArchIso](https://archlinux.org/packages/extra/any/archiso/)
- [PreLoader-Signed (AUR)](https://aur.archlinux.org/packages/preloader-signed) is optional and can be downloaded from other sources. 

## some sources

[Arch wiki on signed boot loaders](https://wiki.archlinux.org/title/Secure_Boot#Using_a_signed_boot_loader)

[Archboot](https://archboot.com/) also seems like a greate ressource if you want to take a look at secure boot. 
These images should be ready to boot, if you dont want to create your own and just want a secure boot ready installation. 


This is an alternative if you dont want to use preloader. 
[shim-signed 15.8+ubuntu+1.59-1](https://aur.archlinux.org/packages/shim-signed)

This might fit better with my purpose.

you can acutally do it with a  [preloader](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface/Secure_Boot#PreLoader) as descriebed in one of the methodes in this [wiki page](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface/Secure_Boot#ISO_repacking)


The tool used to create Archisos is called [archIso](https://wiki.archlinux.org/title/Archiso). The documentation is very helpfull in creating a base iso with what we need. 



## Actual rundown 

the following commands are all exectued under archlinux, under a different distro you would need to clone the tool first. 

since we want to modify the image we will make a copy 
```shell
cp -r /usr/share/archiso/configs/releng/ <dir that you want to work in>
```
I chose releng since I might need extra tooling but you could also slim this down a lot and just use baseline. 

If you want to customize things like hostname and the like you should be doing that in the airootfs, this is the filesystem that gets loaded during the startup as the live system. 


### setting up secure boot
Since we want secure boot on this ISO, and we dont want

The arch wiki has [2 options to enable secure boot](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface/Secure_Boot#Booting_an_installation_medium) you can repack the installation medium to include secure boot. I would use Preloader since I do not care about frequent kernel signing and I dont want to load new keys everysoften. 

This should be patchable to any arch installation.
Thus this step should be execute when the disk is created. 
The only problem Could be with preloader is that the ones that exist in the repos are from 2013 so quite old by now. They will work until 2026. This is when the keys will expire. And the Preloader might not work anymore. [Taken from this blog Post](https://blog.hansenpartnership.com/linux-foundation-secure-boot-system-released/) Since the binarys are also in the linked in the blogpost.

#### Replacing the boot loader 

[We will use this aproach](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface/Secure_Boot#Replacing_the_boot_loader_with_PreLoader) since, this is the easiest. 

this command extracts from the image
```shell
osirrox -indev archlinux-YYYY.MM.DD-x86_64.iso \
	-extract_boot_images ./ \
	-extract /EFI/BOOT/BOOTx64.EFI loader.efi
```

```shell

mcopy -D oO -i eltorito_img2_uefi.img BOOTx64.EFI loader.efi HashTool.efi ::/EFI/BOOT/
```

```shell
xorriso -indev archlinux-YYYY.MM.DD-x86_64.iso \
	-outdev archlinux-YYYY.MM.DD-x86_64-Secure_Boot.iso \
	-map_l ./ /EFI/BOOT/ BOOTx64.EFI loader.efi HashTool.efi -- \
	-boot_image any replay \
	-append_partition 2 0xef eltorito_img2_uefi.img
```



### Further customization 

#### Customizing Pacman to include specific AUR packages
A [list of mirrors ](https://wiki.archlinux.org/title/Unofficial_user_repositories) is here add the one you need to the pacman config in releng/pacman.conf
There you simply need to add the mirror that you need

If no internet repo exists, you could simply add a local mirror with the a snapshot. 

##### Custom Repo
[As described in the Arch wiki](https://wiki.archlinux.org/title/Custom_local_repository#Custom_local_repository)
Also helpfull is this [Artikle on offline installation](https://wiki.archlinux.org/title/Offline_installation)
since it explains how to create a local repo

you will also in the repo you want to do create a [local db](https://bbs.archlinux.org/viewtopic.php?id=187714)

```shell
repo-add -n <name of your package>.db.tar.xz *.pkg.tar*
```
and then have a full path of your installation medium meaning a fully qualified path to this. 



so in the pacman.conf we will append the following option 

```pacman.conf
[<the name of your package>]
file = file:///<path to your file>
```
this should work in theory but I didnt manage to get it to work

Alternative if you want to add a custom repo like blackarch you can simply add it like it is in [Blackarch ISO](https://github.com/BlackArch/blackarch-iso/tree/master), 


```pacman.conf
[blackarch]
Server = https://www.blackarch.org/blackarch/$repo/os/$arch
```

If you intend to to do that you should also have the repo added to your pacman or atleast the keys it is signed with, since those are needed during the inital build of the system. 

The full path is as follows : /airootfs/etc/pacman.d/gnupg/*

###### bypass
If you want to bypass adding the keys to your system you could add the following line to the blackarch config though this is not recomended you do 
```pacman.conf
[blackarch]
SigLevel = Never
Server = https://www.blackarch.org/blackarch/$repo/os/$arch
```

#### Adding boot option to only boot into the intramfs under Systemd boot
[Taken from this Ubuntu Forum post](https://askubuntu.com/questions/1043242/how-do-i-force-ubuntu-to-boot-into-initramfs) but since its the boot process it should not matter.
[Arch has a bit more proper documentation](https://wiki.archlinux.org/title/Systemd-boot#Loader_configuration)
for systemdboot in this case since that is what the archinstaller uses for UEFI boot situations.

[The more advances Standart](https://uapi-group.org/specifications/specs/boot_loader_specification/)
[Systemd-Boot documentation](https://systemd.io/BOOT/)


It appears we need to seet some kernel parameters an incompleate list and how to do it in generall is described here
[Parameter List arch wiki](https://wiki.archlinux.org/title/Kernel_parameters#Parameter_list)


to the releng/efiboot/loader/entries directory add a entry with another number
the sort key setting is entirely optional and can be omited 

```shell
title    InitramFS
sort-key 0<your number give the file>
linux    /%INSTALL_DIR%/boot/x86_64/vmlinuz-linux
initrd   /%INSTALL_DIR%/boot/x86_64/initramfs-linux.img
options  archisobasedir=%INSTALL_DIR% archisosearchuuid=%ARCHISO_UUID% break
```

or you can edit the current boot option and manually add the break to it.

this should create an entry in the boot options to boot to the intiramfs, you should name it apropatily aswell

#### OS information 

Simply copy the /etc/os-release into the airootfs/etc directory, there you can edit it this will also change information about your OS. 


### customizing the initramfs
This is only under very specific circumstances needed and otherwiese should be avoided
The initramfs is managed by [MKinitcpio](https://wiki.archlinux.org/title/Mkinitcpio) in the default iso, there are alternatives but I will use this. 

Some configuration hints can be found in the [Arch wiki further down](https://wiki.archlinux.org/title/Mkinitcpio#Configuration)

In the archiso the specific file can be found under ./relang/airootfs/etc/mkinitcpio.conf.d/archiso.conf
you could also create an entirely new file if you wanted to but then you would need to specify it in ./relang/airootfs/etc/mkinitcpio.d/linux.preset

I recomend you take a look at the documentation, in my usecase I will simply get the required things by using the binaries keyword.

### adding a custom kernel modul into the initramfs and in general 

in this case we want to simply get a new kernel modul into the initramfs for this we need to add the package


### Creating the image 

the command to create the image is as follows 

```shell
sudo mkarchiso -m iso -A <name of your application> -v  -w <path for you to work in > <path where you're files are /releng/ yes this is just seperated by a space>
```

-w should be a location in /tmp since you do not need these files if you want these files to be destroyed after creating the image. And these are mearly temporary files

the -o option optional
when selecting the profile you should not select the profiles.sh (the one that is located in relang) directly just the directory

#### Error <> is missing profiledef.sh


the file is inside of the image. That you are building you should specify to that 

# Creating PXE bootable images
## some sources

- [PXE Booting x86_64 UEFI Clients with Dnsmasq](https://ressman.org/posts/2018-05-06-pxe-boot-up-boards/)
- [ ]

the special problem with PXE boot is, that the initramfs needs to be a seperate file aswell as the bootloader. 
Getting a special bootloader is not a problem especially if you want to secure boot. 


Since this is a problem in generall we will take a look at 
## Unpacking ISO files

since that is a general solution that should work with every Distrobution.
[The arch guid to replacing the boot loader with preloader](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface/Secure_Boot#Replacing_the_boot_loader_with_PreLoader).

In general you want to unpack these iso files to obtain the initramfs so that you can seperate it from the rest of the operating system. 
Another option would be to just build the initrmafs itself, this should also work.This has also been discussed on the  [Arch forums ](https://bbs.archlinux.org/viewtopic.php?id=250438) to some degree. 
# Creating a bootable usb 

[Arch wiki](https://wiki.archlinux.org/title/USB_flash_installation_medium)
for most linux distrobtions its as easy as simply copying the file onto the harddrive, I think for windows ISOs you need to do some extra steps to get them to work. 

```shell 
cp path_to_iso > /dev/sda
```
