
# Some relevant information on text processing 

- [pubs opengroup specification](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap11.html)
- [cannonical or not GNU](https://www.gnu.org/software/libc/manual/html_node/Canonical-or-Not.html)
- [Stackoverflow: Cannonical vs none cannonical](https://stackoverflow.com/questions/358342/canonical-vs-non-canonical-terminal-input)
- [terminos](https://www.man7.org/linux/man-pages/man3/termios.3.html)
- [mksoft terminos](https://www.mkssoftware.com/docs/man5/struct_termios.5.asp)
- 

Some tutorial on how to to deal with text processing can be found [In this note](/langs/c++/IO) Where I am looking at reading none cannonical inputs 



# VT 100 escape sequeneces
[web archive source ](https://web.archive.org/web/20121225024852/http://www.climagic.org/mirrors/VT100_Escape_Codes.html)

clear line : 
```
\33[2K\r
```

Some preprocessor C code that clears lines / below / above cursor
```C
#define CLEARLINE printf("\33[2K\r");
#define CLEARBELOWCURSOR printf("\x1b[0J");
#define CLEARABOVECURSOR printf("\x1b[1J");
```

# Detecting cursor (in a terminal context)

This does not refere to your mouse pointer but rather where your input lies in the terminal 

- [linuxquestions Thread](https://www.linuxquestions.org/questions/programming-9/get-cursor-position-in-c-947833/)
- [conio.h](https://code-reference.com/c/conio.h/wherex) -> a lib that allows wherex and wherey and goto on screen