# Basic commandline Usage 

this explains some basic commandline usage

# common issues 


## qemu-system-x86_64:  : Could not open ' ': No such file or directory

means if you have split the script into multiple lines that somewhere you have a \ with a space behind, or some other syntax error. Recheck all \ that there is no space after them and that they are the last symbol in the line

# Windows 
[A good repo with some good information](https://github.com/infokiller/win10-vm)


## creating a inital virtual maschine 
[A guid for win10](https://pragmaticaddict.com/qemu-win10.html)
[Win11 adds some more complexity](https://k4i.top/posts/windows-11-vm-with-qemu-kvm/)
Though some things can be let out since this is orignally aimed at people wanting to update garmin devices the normal setps can be reproduuced just with small changes

the following command creates the your virtual drive
```shell
qemu-img create -f qcow2  <name of the drive>.qcow2 100G
```

the following command then runs the Virtual maschine 
```shell
#!/bin/sh 

exec qemu-system-x86_64 \
    -enable-kvm -cpu host -m 16G \
    -drive file=<name of your drive>,if=virtio \
    -device  virtio-tablet \
    -rtc base=localtime \
    -net nic,model=virtio-net-pci -net user,hostname=win10 \
    -monitor stdio \
    -name "win10" \
    -display gtk,grab-on-hover=on \
    
	"$@"
```
the last line "$@" allows more commandline parameters
all of the commands are explained in the top linked article 
excep the -tpm modul this is from the faking a TPm setion 


```sh
sh <script from abouve>.sh -boot d -drive file=<path to windows iso>.iso,media=cdrom 
```

### Win 11
the above mentionted will not work wit win 11

the abouve should work with win10 and below but windows 11 brings another challange so we have some problems. 

That are mainly TPM 2.0 and Secure boot requirements. You can disable/bypass them but that is not what you usually want if you want to test something in that regard
```shell
#!/bin/sh 

exec qemu-system-x86_64 \
    -machine q35,smm=on,accel=kvm \    
    -enable-kvm -cpu host -m 16G \
    -drive file=<name of your drive>,if=virtio \
    -device  virtio-tablet \
    -rtc base=localtime \
    -net nic,model=virtio-net-pci -net user,hostname=win11 \
    -monitor stdio \
    -name "win11" \
    -display gtk,grab-on-hover=on \
    -chardev socket,id=chrtpm,path=/tmp/emulated_tpm/swtpm-sock \
    -tpmdev emulator,id=tpm0,chardev=chrtpm -device tpm-tis,tpmdev=tpm0 \     
    -drive if=pflash,format=raw,readonly=on,file=<Path to 2.secboot.4m.fd> \
    -drive if=pflash,format=raw,file=<Path to OVMF_VARS.4m.fd> \

	"$@"
```

THe Following command needs to be run in a seperate terminal window everytime before you start the VM to create a new TPM modul
```shell
mkdir /tmp/emulated_tpm
swtpm socket --tpmstate dir=/tmp/emulated_tpm --ctrl type=unixio,path=/tmp/emulated_tpm/swtpm-sock --log level=20 --tpm2
```
the following command differs slightly from the one mentioned abouve in that it removed the mention to what kind of media it is, this seems to create a problem with the UEFI. 


```sh
sh <script from abouve>.sh -boot d -drive file=<path to windows iso>.iso,media=cdrom
```


## Windows having problems with the drive to install to 

I think this might be the issue since even when adding registry keys to bypass the other checks it still did not work. But the DIsk did show up and even after formating it did not work


## Faking a TPM

the following additional packages need to be installed 
 
this is to fake a TPM modul to satisfy the windows 11 Instalation aswell as for research purposes.

A Guid on [TPM2 Device emulation](https://tpm2-software.github.io/2020/10/19/TPM2-Device-Emulation-With-QEMU.html) 


run the following command to start emulating a tpm 
It needs to be run every time you want to start the VM if the command isnt already running

```shell
mkdir /tmp/emulated_tpm
swtpm socket --tpmstate dir=/tmp/emulated_tpm --ctrl type=unixio,path=/tmp/emulated_tpm/swtpm-sock --log level=20 --tpm2
```

this creates a temporary directory for the tpm state and then opens a socket for wich the tpm can communicate with the VM.

the the following option needs to be added to the VM script 
as it is mentioned before 
```shell
    ...
  -chardev socket,id=chrtpm,path=/tmp/emulated_tpm/swtpm-sock \
  -tpmdev emulator,id=tpm0,chardev=chrtpm -device tpm-tis,tpmdev=tpm0 \

```


## Faking secure boot and UEFI

The packages you need to have fake secure boot are 

- swtpm
- edk2-ovmf

some sources used to understand this 
[Superuser Question](https://superuser.com/questions/1660806/how-to-install-a-windows-guest-in-qemu-kvm-with-secure-boot-enabled)
[Securish boot with qemu](https://www.labbott.name/blog/2016/09/15/secure-ish-boot-with-qemu/)



### UEFI 

first we will create a backup of the firmware files that we installed with edk2-ovmf. 
Since I do not want them changed, though they should not change but this also shortens our paths. 


you might need to adjust these paths since they where used under arch linux 
```shell
cp /usr/share/edk2/x64/* ./UEFI/
```
Theoretically you only need to copy the VARs file since that is the  only thing being [changed](https://wiki.debian.org/SecureBoot/VirtualMachine)
 

the add the following 4 lines to your script 
```shell 
...   
    -drive if=pflash,format=raw,readonly=on,file=<Path to OVMF_CODE.secboot.4m.fd> \
    -drive if=pflash,format=raw,file=<Path to OVMF_VARS.4m.fd> \
    -machine q35,smm=on,accel=kvm \
```

##### Display output not active

you forgot to add the line 

```shell
-machine q35,smm=on,accel=kvm \
```
most likely 

##### Failed to load Boot0001 "UEFI Misc Device" from ...
[Proxmox Forums](https://forum.proxmox.com/threads/guest-has-not-initialized-the-display-yet-on-new-ovmf-vms-after-update-to-7-0-13.98179/) 

Just wait to get into the UEFI shell 
once you are there type the following commands :


[Command Help](https://hatchjs.com/efi-shell-version-2-70-commands/)
[A helpfull guid to fixing this problem and related ones ](https://mricher.fr/post/boot-from-an-efi-shell/)

if you wait long enought you get into a uefi boot menue where you can should be able to select the boot drive but it does not find any drives.
It will find the drive if you stop specifing what kind of drive it is. This seems to be the cause of my problems so. You will need to remove it both on the cd with the ISO and just label it as a generic drive
aswell as on the drive you want to write to for them to show up in the UEFI firmware. 

if it does not automactically boot simply type exit into the uefi shell this will take you to a menu, where you can manually boot from a specific device. 
Otherwiese you can also adjust the boot order manually, either from the shell or from the menu. 
[This also says that you might need to adjust the boot order](https://github.com/virtio-win/kvm-guest-drivers-windows/wiki/Driver-installation#installing-drivers-required-for-windows-installation-such-as-hard-disk-drivers)


## Driver Issues 
Was caused by me removing the media=cdrom from the boot medium, this is quite important as it apears. Otherwiese it will not detect it as the correkt drive.


# Debugging no Internet in your VM 

since this is what is occuring to me these are the steps I took to fix it. 

firstly I check what configuration appears on the guest 
with 
```shell
ip a
```
the ethernet device shows up. 
then I check the bridged interface 

```shell
# tcpdump -i virbr0
```

after further tracking things down I noticed this seems to be an issue with ntftables. 
[arch forum](https://bbs.archlinux.org/viewtopic.php?id=261670)

since when deaktivating nftable servce the VM got internet. 

since that forum post annoyingly does not include.
luckly [artix forum](https://forum.artixlinux.org/index.php/topic,7203.0.html) also has a solution to this problem 
so you have to add 
```
iifname virbr0 accept
```
to the /etc/nftables.conf config file. and then Rebooting fixed the issue (dont forget to reenable the nftables service )

Though that allowed me to forward dhcp requests and atleast connect though i had no internet so i simply switched to ufw. 
and followed the given command in the artix post. 



# Hypervisor Details 

choosing a chipset under Virtual Machine Manager

https://wiki.qemu.org/images/f/f6/PCIvsPCIe.pdf
https://wiki.qemu.org/images/4/4e/Q35.pdf

The current supported optiosn are Q35 and i440fx

Q35 is the newer option that has support for some newer features, like secure boot.
While i440fx seems to be a more legacy option that is needed when you want to boot legacy systems like windows xp/ 2000 or older. 
