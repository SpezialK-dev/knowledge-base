# Sources
- http://x86asm.net/articles/uefi-programming-first-steps/
- https://github.com/louisbyun/Creating-a-Simple-UEFI-Application-with-EDK2-and-Visual-Studio-2022-A-Beginner-s-Guide
- https://wiki.osdev.org/GNU-EFI
- https://wiki.osdev.org/UEFI_Bare_Bones
# Actually building your first UEFI application



## Using GNU-EFI

firstly build your the gnu efi library
```shell
$ git clone https://github.com/ncroxon/gnu-efi.git
$ cd gnu-efi
$ make
```


Building your own binary should have atleast the following code 

```c
#include <efi.h>
#include <efilib.h>

EFI_STATUS
EFIAPI
efi_main (EFI_HANDLE ImageHandle, EFI_SYSTEM_TABLE *SystemTable)
{
  InitializeLib(ImageHandle, SystemTable);
  Print(L"Hello, world!\n");
  return EFI_SUCCESS;
}
```


### Using the OS Wiki way (in a seperate repo)
then once you have done that you have to add the following flags to compile it 

```shell
$ gcc -Ignu-efi-dir/inc -fpic -ffreestanding -fno-stack-protector -fno-stack-check -fshort-wchar -mno-red-zone -maccumulate-outgoing-args -c main.c -o main.o
```

after that the linking has to be done
```shell
$ ld -shared -Bsymbolic -Lgnu-efi-dir/x86_64/lib -Lgnu-efi-dir/x86_64/gnuefi -Tgnu-efi-dir/gnuefi/elf_x86_64_efi.lds gnu-efi-dir/x86_64/gnuefi/crt0-efi-x86_64.o main.o -o main.so -lgnuefi -lefi
```

this is a shorter version of [OS wiki](https://wiki.osdev.org/GNU-EFI)

but the following script should also work, and is what I am currently using but the paths where not working 

```sh
$ gcc main.c                             \
      -c                                 \
      -fno-stack-protector               \
      -fpic                              \
      -fshort-wchar                      \
      -mno-red-zone                      \
      -I /path/to/gnu-efi/headers        \
      -I /path/to/gnu-efi/headers/x86_64 \
      -DEFI_FUNCTION_WRAPPER             \
      -o main.o

$ ld main.o                         \
     /path/to/crt0-efi-x86_64.o     \
     -nostdlib                      \
     -znocombreloc                  \
     -T /path/to/elf_x86_64_efi.lds \
     -shared                        \
     -Bsymbolic                     \
     -L /path/to/libs               \
     -l:libgnuefi.a                 \
     -l:libefi.a                    \
     -o main.so

$ objcopy -j .text                \
          -j .sdata               \
          -j .data                \
          -j .rodata              \
          -j .dynamic             \
          -j .dynsym              \
          -j .rel                 \
          -j .rela                \
          -j .reloc               \
          --target=efi-app-x86_64 \
          main.so                 \
          main.efi
```
As taken from [OS WIKI- UEFI](https://wiki.osdev.org/UEFI)

a few notes on the paths atleast when running make in the `gnuefi` the paths are different I did not check with the other paths but these ones are plainly wrong. 


in the inital compilation  phase, the paths shoould look like this, since headers does not exist anymore. 
```shell
-I ../gnu-efi/inc        \
-I ../gnu-efi/inc/x86_64 \
```

in my case the folder strucutre is as follows 

```
/
|- gnu-efi (git repo)
|- your projekt 
	|- compilation script
```

hence the .. infront of everything 


in the second step the paths are as follows 
```
../gnu-efi/x86_64/gnuefi/crt0-efi-x86_64.o
../gnu-efi/gnuefi/elf_x86_64_efi.lds
```

the last thing needed for the two libs 
```
../gnu-efi/x86_64/
```
here you will have to look into the subfolers `lib` as well as `gnuefi` to find the files I simply copyed them into the above mentioned path.

## alternative GNU way

if you dont want to have a separate repo, you can simply add your efi binary to the apps directory and modify the Makefile makros a bit and it should also be automatically build. 
# Creating a disk image 

also based on  [OS wiki - UEFI](https://wiki.osdev.org/UEFI)


## virtual FAT image 

1. create directory 
2. add it as a drive 

```shell
$ qemu-system-x86_64 -cpu qemu64 \
  -drive if=pflash,format=raw,unit=0,file=path_to_OVMF_CODE.fd,readonly=on \
  -drive if=pflash,format=raw,unit=1,file=path_to_OVMF_VARS.fd \
  -drive format=raw,file=fat:rw:esp \
  -net none
```

under arch linux the path to OVMF code is as follows :

```shell
/usr/share/OVMF/x64/OVMF_CODE.4m.fd
```


```shell
qemu-system-x86_64 -cpu qemu64   -drive if=pflash,format=raw,unit=0,file=/usr/share/OVMF/x64/OVMF_CODE.4m.fd,readonly=on   -drive if=pflash,format=raw,unit=1,file=./OVMF_VARS.4m.fd  -drive format=raw,file=fat:rw:esp   -net none
```