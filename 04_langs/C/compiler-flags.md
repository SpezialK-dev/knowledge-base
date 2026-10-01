

# nostdlib nostdinc

https://stackoverflow.com/questions/30790392/how-can-i-use-stdio-and-stdlib-while-compiling-with-nostdlib-and-nostdinc-flags

> `-nostdlib` tells gcc to not include tells the compiler to not include the standard libraries, one of them being glibc (the GNU libc). `-nostdinc` tells the compiler not to use the standard paths for header files. The functions with prototypes in stdlib.h and stdio.h are part of libc which you have told the gcc not to use. Therefore you will need to implement them yourself or include a libc explicitly (some common ones to use for custom operating systems are newlib and uClibc). Which approach is easier depends on the functions being used. I should add that at this point if you are trying to compile an operating system and an application, even a static application, you might want to look into using a cross-compiler ([http://wiki.osdev.org/GCC_Cross-Compiler](http://wiki.osdev.org/GCC_Cross-Compiler)) and if you can get that working the next step is your own toolchain which includes a libc ([http://wiki.osdev.org/OS_Specific_Toolchain](http://wiki.osdev.org/OS_Specific_Toolchain)).
>
>If implementing the necessary functions is not an option than your best bet is to use a full toolchain including your own libc. You cannot simply use the GNU libc as part of what libc is is a wrapper around the system calls and you would have to have a kernel that has a system call interface that is the same as the host operating system. Note that part of the process of porting a libc to your operating system is implementing the system calls. For newlib this includes but is not limited to write, read, open, close, fork and exec. If your application does not use these system calls you can simply implement dummy versions.
- missimer

these flags are used when cross compiling to not pollute with your own c libs 


## nostdlib

>tells gcc to not include tells the compiler to not include the standard libraries, one of them being glibc (the GNU libc).


## nostdinc

>tells the compiler not to use the standard paths for header files. The functions with prototypes in stdlib.h and stdio.h are part of libc which you have told the gcc not to use

## fixing 

