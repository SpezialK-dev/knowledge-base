

getting the Logitech G15 to run under arch linux the normal keyboard works without any drivers and it has 7 Key rollover as it appears from my testing so does the audio stuff mostly.

# TODO

- [ ] Fix the AUR packages 
# Usage

you can use 

g15term to execute a programm that should be showen on there 
g15tailf to show text from a textfeil 

you can have multible programms running on the same thing.
# Drivers 
[user manual](https://www.manua.ls/logitech/g15/manual)

[Arch wiki](https://wiki.archlinux.org/title/Logitech_Gaming_Keyboards)
^ contains more information on what is needed and what works

The important ones are 
- g15deamon (none of the packages work)
- g15macro
- g15status()
-  g15utils

currently the normal deamon package has the wrong url so we could compile it ourself with the git package [g15Deamon-git](https://aur.archlinux.org/packages/g15daemon-git)


This throws us an error 
```shell
CDPATH="${ZSH_VERSION+.}:" && cd . && aclocal-1.16 
/bin/sh: line 1: aclocal-1.16: command not found
```

to fix this we need to we need to install the package 
[automake](https://archlinux.org/packages/core/any/automake/)
sadly this installs the newer version

## How to install this without using the aur packages 


### compiling it yourself 
since all of this does not work we will update this package 

[git repo ](https://gitlab.com/menelkir/g15daemon/)

to compile this yourself we will need to type the following commands 

```shell
$ git clone https://gitlab.com/menelkir/g15daemon.git
$ yay -Syu libg15 libg15render # installes the needed dependencies on Arch linux
$ libtoolize && aclocal && autoconf && automake && ./configure 
$ sudo make install
```

#### My issues and troubleshooting
this is not needed and more if you have similar problems but the above works and has been sort of tested^^^

I bumped up the version for aclocal from  1.16 to 1.17.  same needs to be done for automake. and after changing those things 

`./configure` needs to be run again
libtoolize && aclocal && autoconf && automake && ./configure 
weirdly enough automake the automake version gets overwriten everytime we run ./configure I have not figured out why but so you need to replace it and then run make install

afterwards the following error occurs 

```shell
make[1]: Entering directory '/home/spezialk/Documents/git/g15daemon/libg15daemon_client'
/bin/sh ../libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I. -I..   -DDATADIR="\"/usr/share\"" -DPLUGINDIR=\"/usr/lib/g15daemon/3.0.1/plugins\" -I/usr/include  -g -O2 -MT g15daemon_net.lo -MD -MP -MF .deps/g15daemon_net.Tpo -c -o g15daemon_net.lo g15daemon_net.c
libtool: Version mismatch error.  This is libtool 2.4.6.42-b88ce-dirty, but the
libtool: definition of this LT_INIT comes from libtool 2.5.3-dirty.
libtool: You should recreate aclocal.m4 with macros from libtool 2.4.6.42-b88ce-dirty
libtool: and run autoconf again.
make[1]: *** [Makefile:438: g15daemon_net.lo] Error 63
make[1]: Leaving directory '/home/spezialk/Documents/git/g15daemon/libg15daemon_client'
make: *** [Makefile:577: install-recursive] Error 1
```

to fix this we run the following commands taken from this [stackoverflow post](https://stackoverflow.com/questions/58565768/libtool-version-mismatch-error-2-4-6-expected-vs-2-4-6-42-b88ce-actual-ac)


```shell
libtoolize && aclocal && autoconf && automake && ./configure 
```

```shell
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -I.. -DDATADIR=\"/usr/share\" -DPLUGINDIR=\"/usr/lib/g15daemon/3.0.1/plugins\" -I/usr/include -g -O2 -MT g15daemon_net.lo -MD -MP -MF .deps/g15daemon_net.Tpo -c g15daemon_net.c  -fPIC -DPIC -o .libs/g15daemon_net.o
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -I.. -DDATADIR=\"/usr/share\" -DPLUGINDIR=\"/usr/lib/g15daemon/3.0.1/plugins\" -I/usr/include -g -O2 -MT g15daemon_net.lo -MD -MP -MF .deps/g15daemon_net.Tpo -c g15daemon_net.c -o g15daemon_net.o >/dev/null 2>&1
mv -f .deps/g15daemon_net.Tpo .deps/g15daemon_net.Plo
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -g -O2 -version-info 3:0  -o libg15daemon_client.la -rpath /usr/lib g15daemon_net.lo  -lpthread -lm -lg15render -lg15 
libtool: link: gcc -shared  -fPIC -DPIC  .libs/g15daemon_net.o   -lpthread -lm -lg15render -lg15  -g -O2   -Wl,-soname -Wl,libg15daemon_client.so.3 -o .libs/libg15daemon_client.so.3.0.0
libtool: link: (cd ".libs" && rm -f "libg15daemon_client.so.3" && ln -s "libg15daemon_client.so.3.0.0" "libg15daemon_client.so.3")
libtool: link: (cd ".libs" && rm -f "libg15daemon_client.so" && ln -s "libg15daemon_client.so.3.0.0" "libg15daemon_client.so")
libtool: link: ar cr .libs/libg15daemon_client.a  g15daemon_net.o
libtool: link: ranlib .libs/libg15daemon_client.a
libtool: link: ( cd ".libs" && rm -f "libg15daemon_client.la" && ln -s "../libg15daemon_client.la" "libg15daemon_client.la" )
make[2]: Entering directory '/home/spezialk/Documents/git/g15daemon/libg15daemon_client'
 /usr/bin/mkdir -p '/usr/lib'
 /bin/sh ../libtool   --mode=install /usr/bin/install -c   libg15daemon_client.la '/usr/lib'
libtool: install: /usr/bin/install -c .libs/libg15daemon_client.so.3.0.0 /usr/lib/libg15daemon_client.so.3.0.0
/usr/bin/install: cannot create regular file '/usr/lib/libg15daemon_client.so.3.0.0': Permission denied
make[2]: *** [Makefile:375: install-libLTLIBRARIES] Error 1
make[2]: Leaving directory '/home/spezialk/Documents/git/g15daemon/libg15daemon_client'
make[1]: *** [Makefile:568: install-am] Error 2
make[1]: Leaving directory '/home/spezialk/Documents/git/g15daemon/libg15daemon_client'
make: *** [Makefile:577: install-recursive] Error 1
```

the fix 


```shell
sudo make install
```

since we want to install the package

### testing if it is installed

```
g15daemon -k
```

if not running returns just start it. 

After running it it should start up by the g15deamon with sudo. if this works the display should now show your system time instead of the normal Logitech logo. 

## Further problems 
since we compiled the deamon ourself we need to also compile the other things ourself

# Compiling g15 utils
 [git repo](https://gitlab.com/menelkir/g15utils)
This is so that you can create your own things for the display 


here we have the same problems so we need to run 

```shell
libtoolize && aclocal && autoconf && automake && ./configure 
```

this fixes all of our problems also for the above 
then we can run `sudo make install`

## Usage 

 ```shell
 g15tailf <filename>
 ``` 


to run a program programs need to not exit. bc once this exists this stop showing
```shell
g15term <program>
```

# Where are the most memory leaks ?

atleast from the quick crash that I encoutnered a lot of the probelms occured in [this repo](https://github.com/vividnightmare/libg15render) specicially in screen.c and text.c
