rr is a time traveling debuger, that mostly works on Intel CPU's though there are some workaround for ZEN CPU's


Links : 

https://rr-project.org/
https://github.com/rr-debugger/rr

# Compiling RR 
https://github.com/rr-debugger/rr/wiki/Building-And-Installing


under Arch linux there is a aur package 
https://aur.archlinux.org/packages/rr

though I went through the compilation process myself



some of the packages required where 

- capnproto
- python-ptyprocess
- python-pexpect
- lldb
- make
- cmake
- gdb
- perf
- zlib
- git 
- ninja
- pkg-config
- bash
- python
- gcc-libs


then you simply follow the 


```shell
git clone https://github.com/rr-debugger/rr.git
mkdir obj && cd obj && cmake -DCMAKE_BUILD_TYPE=Release ../rr
```
and then 
```shell
make -j8
sudo make install
```

for up to date instructions always look at the uptop linked github page


# Usage 


# Zen Patches and workarounds 