# Arch linux

## Sources 

- https://wiki.archlinux.org/title/Creating_packages
- https://wiki.archlinux.org/title/Arch_packaging_standards
- https://wiki.archlinux.org/title/Building_in_a_clean_chroot
- https://wiki.archlinux.org/title/PKGBUILD

## guid
A small guid to slim down the things you need to read to create arch linux packages. You should still read the [Arch packaging standarts](https://wiki.archlinux.org/title/Arch_packaging_standards) Since those are actually important  if you want to contribute to the AUR. 

```shell
depends=('linux-headers')
makedepends=('gcc' 'make' 'git')
```

depends is what the program needs to activly run and makedepends are the ones that are needed to compile the program and that might not be needed to compile 

```shell
pkgver=1.6.0
pkgrel=1
```

pkgver is the version that the version of the software globally
pgrel ist what version this pkgfile has. meaning with every change to the file you should increment this. 


Aktually take a look at the liscences
```shell
license=('GPL-2.0-only')
```



namcap is your friend in trying to create packages it tests the packages 

## Specifics when packaging kernel packages 
Some of the paths are taken from [This Nvidia Kernel driver](https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=nvidia-340xx) since it was not very straigh forward to figure out how else to do it.


The correct path for the package where to copy the package into after compiling it.

[Some guid on actually compiling this thing](https://wiki.archlinux.org/title/Compile_kernel_module)
 
importantly so that you later can load the module with a simpel modprobe
```shell
buidl)({
	#.. rest of the build process
	zst ./<your kernel package>.ko

}


package(){
	_kernver=$(</usr/src/linux/version)	
	cd $pkgname	
	install -Dvt "$pkgdir/usr/lib/modules/$_kernver/updates" -m0644 <kernel_package>.ko.zst
		
}
```


apparently it does not work when trying to put it into /lib/..
but putting it into /usr/lib/.. works.


# Using devtools