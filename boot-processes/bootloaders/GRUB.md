
# deactivating signature checking in grub 

https://www.gnu.org/software/grub/manual/grub/html_node/Using-digital-signatures.html

with 

```
set check_signatures=no
```
typed into the grub shell theoretically you can deactivate checking the signatures embedded into the grub

# Getting into grub shell

if you have no timeout and you dont see a grub screen but you know you can edit the grub config simply press F8+any other key [Source](https://srobb.net/rhboot.html). Atleast it worked under Fedora. 


# Building grub from source 
(written since there is no other good documentation out there on how to build the repo)
firstly clone the repo I am simply cloning from the [github mirror](https://github.com/rhboot/grub2) this apparently includes some patches for Fedora / RHL specifically. 
Though I do recommend getting one of the releases from the release tab since those usually compile a lot better, and have fewer compilation errors. 


## installing all prerequisite packages 

under a debian based distro the needed packages are the following ones 

This list ist partly taken from [here](https://www.dotlinux.net/blog/grub-compile-from-source-on-linux/) but the list there was missing some dependencies so I added them here

```
build-essential
bison
flex
libelf-dev
autopoint
libssl-dev
python3
autoconf
automake
pkg-config
git 
wget
```

once this is done you can get to the setup 

## Setup 

for that firstly run 

```shell
cd grub2
sh ./bootstrap
```

one can ignore the `cd grub2` command if they are already in the directory for grub2

if you want to build for an efi system you will need the following flags otherwise it will not work 

```shell
sh configure --with-platform=efi --target=x86_64
```


## compilation 

simply run 

```shell
make j4
```

(the j4 is there to speed up the compilation since its now multi threaded )

