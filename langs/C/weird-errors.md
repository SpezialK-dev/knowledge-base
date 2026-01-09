

# fatal error: stddef.h: No such file or directory

```shell
/usr/include/efi/x86_64/efibind.h:99:10: fatal error: stddef.h: No such file or directory
   99 | #include <stddef.h>
```

is the full error code. 

https://stackoverflow.com/questions/31600600/compilation-error-stddef-h-no-such-file-or-directory

There are two suggested things that could be the case 

1. my gcc-core and gcc-g++ package are out of sync and not the same version 
2. apparently this can also be caused by the [-nostdinc](langs/C/compiler-flags.md) flag


the first one was not the the problem in my case, since both packages where up to date, as well as the same version. 

simply the -nostinc fixed and allowed it to compile 