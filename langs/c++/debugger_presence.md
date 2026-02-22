
# How to detect if a debugger is presence 

https://en.cppreference.com/w/cpp/utility/is_debugger_present.html

on newer c++ compilers https://en.cppreference.com/w/cpp/utility/is_debugger_present.html should be used

```c++
bool is_debugger_present()
```
simply needs to be called to know if a debugger is attached or not. but this only works since c++ 26 so it might not work on older compilers. 


## under linux 

the following C code works under linux
```c
#include <sys/ptrace.h>

if (ptrace(PTRACE_TRACEME, 0, NULL, 0) == -1)
  printf("traced!\n");
```

taken from [stackoverflow](https://stackoverflow.com/questions/3596781/how-to-detect-if-the-current-process-is-being-run-by-gdb)
