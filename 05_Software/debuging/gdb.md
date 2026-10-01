# Some generall sources

- [IEEE beginer guid to gdb](https://cs-ieee-ist.github.io/cs-essentials/content/gdb/Introduction)
- 


# general usage

Its advantagous if you compile your software with the -g option 

## basic commands 

these are meant to be typed in the gdb shell, I will not be talking about any frontends

`run` runs the programm
`break main.c:5` sets a breakpoint in line 5 in the file main.c
`print a` prints the value of a at the current time (usefull when encountering a breakpoint)
`next` if we want to execute the current line and stop at the next one we can use this command
`continue` continues the execution of the programm until a breakpoint is hit
`list` shows the next few lines of code 
`file <your programm>` loads a programm if gdb is already running 


## attaching to a pid to debug a running process
[Stackoverflow: using gdb with running process](https://stackoverflow.com/questions/2308653/can-i-use-gdb-to-debug-a-running-process)


`attach <PID>` can help you with that if gdb is already running. 
or you can use it with 
```shell
gdb -p PID
```
with PID being the Process identifyer. 



