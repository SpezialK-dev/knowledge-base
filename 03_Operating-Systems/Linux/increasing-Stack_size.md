
how do you increase the stack size of an application 

- https://superuser.com/questions/1858679/increase-stack-size-for-a-specific-process-on-linux
- https://www.ansaurus.com/question/2275550-change-stack-size-for-a-c-application-in-linux-during-compilation-with-gnu-compiler


this is mainly out of interest 

apparently it is possible to increas stack size in multiple ways


## compiler options 

```

Stack reserve size

- -Wl,–stack,419525
  ```
https://caiorss.github.io/C-Cpp-Notes/compiler-flags-options.html 
its also apparently a thing you can specify when compiling. 