
# Reading files 
an [GeeksforGeeks](https://www.geeksforgeeks.org/c-program-to-read-contents-of-whole-file/) refernce


```c
#include <stdio.h> 
#include <stdlib.h>   
//...

FILE * filptr = fopen("pathToFile", "R")

while ((ch = fgetc(filptr)) != EOF)
	printf("%c", ch);

fclose(filptr)
//...
```
this reads out a single character though you could also use  fgetc() to read an entire line
fread() can read entire blocks of a file.  


if you want to read a long string from input you might also want to use fgets / fopen 


# Memset 

if you are dealing with strings that are empty at the beginning of and that gets filled you might encounter weird parts being printed when changing. 

In my case when doing dynamic input and writing that to a c-String I encountered, that garbage was inside the input this was because of the what was previously in the string. To prevent this from happening again, I initially when creating the string memset it with 0 which prints to nothing. 

```c
memset(&input, 0,input_length*sizeof(char))
```

this then fills the array with its length


# Defining Structs 

the correct way to define a struct under c is the following way 

```c

typedef struct structname_{
	//data of struct 
} structname;
```