

if you want to overwrite functions, in a dynamically linked application, you can do the following. 

you can compile the lib with the following commands
```shell
gcc -c -fPIC hook.c -o hook.o
gcc -shared -o hook.so hook.o
```

```shell
export LD_PRELOAD="./hook.so"
```

and then you simply execute the application with the new functions in your lib overwriting.

one such example for the `time()` call would be the following code this takes input from stdin

```c
#include "hook.h"

//very simple
long int time(long int i){
    printf("Modified time funk was called\n");
    long time_value = 0;
    scanf("%ld", &time_value);
    return time_value;
}
```