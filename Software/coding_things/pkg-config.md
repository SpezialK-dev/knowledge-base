
[DreeDesktop](https://www.freedesktop.org/wiki/Software/pkg-config/)


if you have a package and are building under different Linux derivatives sometimes different flags are needed. 
to find that out run the following commands on distro to find out what flags you need to add to your build command for that distro 
```shell
pkg-config --cflags <lib you want to use>
```

```shell
pkg-config --libs <lib you want to use >
```

if you want to find out what libs you have installed one can use `pkg-config --list-all` that returns all libs installed on the system 