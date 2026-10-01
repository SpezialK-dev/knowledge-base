# Introduction into the topic 
this is based on this learning thing

I will include more sources 

## sources 
- https://learnasm.kb1rd.net/#/learn/ 
- https://www.tutorialspoint.com/assembly_programming/index.htm
- https://github.com/mschwartz/assembly-tutorial
- https://asmtutor.com/

# How computers work

**Instructions** is the way that computers know what calculation is next in line to be processed

first instruction 

## halt instruction 

*halt* -> halts the program execution

rules : one instruction per line
the halt intstruction is important cause it signales the computer to stop reading the memory. If you dont specify this the computer could continue reading and you would have an out ouf bounds read which is dangerous.
Halt tells the computer where the end of the program is.

## Data and move instructions

Registers hold only a single number. -> only 7 out of 8 registers are avialable for me. the 8th register is where the next instruction is being stored. 

r0, r1, r2, r3, r4, r5, r6 -> hold data
r7 -> holds the next instruction


### mov instruction 
*mov* is the instruction used to put data into a register.
move needs a destination and a source. 

destination comes first so puting the number ten into the register r1 would be archived with the following code. 

```asm
mov r1 #10
``` 

### immediate

are numbers that are directly avilable to the computer 
