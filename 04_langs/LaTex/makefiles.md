# How to create makefiles 


## Rubber
If you want to create a makefile with latex, it creates some problems 
you will need to fully know all the steps you want to do. To have a simpel file where you dont want to do a whole lot. You should use [rubber](https://launchpad.net/rubber/). 
This makes it quite simpel to create a simpel makefile.


```makefile 

func: 
	rubber --pdf texfile
```

as simple as this and you have a basic working makefile 

## LatexMk
After some looking around and some testing if you are using any form of biblography just use latexmk 
it just works better 
the command is as follows
```
	func: latexmk -pfd texfile
```

the paths are a bit different with this one and it detectes them from the top level but ohter than that it behaves the same as rubber. I just seems to work better 
It might also work well with rubber since there where still some issues when I switched but from latexmk seems a bit better it just creates more temp files it appears but it works just as well 
and is also easily scriptable. 
And it comes with the AUR texlive install. 
