
# For C/C++ based langs

depending on OS you need to install the mingw package for that distro.
https://linuxvox.com/blog/manual-for-cross-compiling-a-c-application-from-linux-to-windows/

`x86_64-w64-mingw32-c++` will be the name of the compiler binary to then use instead of simply calling g++.




## Pre processor code to only do things dependent of target 

https://stackoverflow.com/questions/430424/are-there-any-macros-to-determine-if-my-code-is-being-compiled-to-windows


some compilers offer a option to know the build enviroment and usually if targeting windows its 

``__WIN32__``

```c
#if defined (__WIN32__)
  // Windows stuff
#endif

```

or you can also simply pass it to the compiler using 

with the -D flag (under gcc)
```shell
`g++ -D __WIN32__ yourfile.c`
```