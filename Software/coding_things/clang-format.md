
[Clang Format Style ](https://clang.llvm.org/docs/ClangFormatStyleOptions.html) 

a tool you could use is [clang format configurator](https://clang-format-configurator.site/).

# not formating a pice of code 
```c++
int formatted_code;
// clang-format off
    void    unformatted_code  ;
// clang-format on
void formatted_code_again;
```


# using a .clangformatfile

```shell
clang-format -style=file:.clang-format src/*
```
will format everything based on a given .clang-format file this will print everything into stdout

```shell
clang-format -style=file:.clang-format -i src/*
```

will replace the things inplace

```shell
clang-format -style=file:.clang-format -i src/*.cpp include/*.h test/*.cpp
```



