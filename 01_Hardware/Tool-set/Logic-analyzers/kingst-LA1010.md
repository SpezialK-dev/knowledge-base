
its a cheapo logic analyzer from amazon they only have  a weird propriatary software. its first a gzip archive and then a tar archive for whatever reason.

# Goals 
- either get it to work with sigrock or seleae. Either of those two. I probably have a bigger chance with the first one

# recomended packages

- https://aur.archlinux.org/packages/pulseview-git
- https://aur.archlinux.org/packages/sigrok-cli-git
I recomend you use these since these work together and the other ones does not you need to compile them yourself but they are somewhat up to date. its best to install both packages. These work in tandem (pulseviewer us optional).

## Some sources for other lost souls 
- [EEV blog ](https://www.eevblog.com/forum/beginners/pulseview-kingst-la1010/) where someone has the exact same question
- [linked article](https://www-foroelectro-net.translate.goog/herramientas-f27/analizadores-logicos-kingst-la-xxxx-y-sigrok-pulse-t474.html?_x_tr_sl=es&_x_tr_tl=en&_x_tr_hl=es) as a solution but its all in spanish
- [sigrok webpage](https://www.sigrok.org/wiki/Kingst_LA_Series) the current device is named as untested

# Installation 
## TLDR version
1. add the virtual driver rules to the /etc/udev/rules.d/  directory (can be found in firmwares folder)
```shell 
 cp 99-Kingst.rules /etc/udet/rules.d
``` 
2.  put the the Sigfiles from the zip (kingst-la-Sig-files.zip) into the $HOME/.local/share/sigrok-firmware folder or another path as specified in the docs
3. restart the pc (optional)
4. plug in device and it should work now

The open linked arch packages should be used or you should compile it yourself with the appimages I have not tried it.

## Longer guid where also some trubleshooting is included
we are trying to in install it for [sigrok](sigrok.md) since that has a higher chance of working since this isnt a seleae clone. 


>first we added the virtaul driver to the /etc/udev/rules.d/ directory
```shell
cp 99-Kingst.rules /etc/udet/rules.d
```

this creates the following very sketchy lsusb device
the one without a name
```
Bus 001 Device 012: ID 77a1:01a2  
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
```
great.... well atleast its not recognized as a serial device IG?


So it you either use the udev rules which I already did and then it should work. It somewhat works, I only have to figure out how to add it now. Also you need some firemware files these are later included in this document
and its inside of the firmwares folder. Next Issue where to put these files. This has not really helped. but it should work so I Will simply extract them myself


## Extracting bitstreams yourself

not needed just download the files provided  in this repo.

> to aquire the git repo clone the following git link 
> 
```shell
git clone git://sigrok.org/sigrok-util
```
the you find the script under 
/sigrock-util/kingst-la you then run the python script 

```
python3 sigrok-fwextract-kingst-la2016 <path to kingstVIs firmware>
```

```shell
./sigrok-fwextract-kingst-la2016 KingstVIS/KingstVIS
```

the files extracted from there need to be placed in on of the follwing paths.
> These files can be found in the under ./firmwares/kingst-la-Sig-files.zip 

as quoted from the web page 

```
- Inside the XDG_DATA_HOME directory: **$HOME/.local/share/sigrok-firmware**
- Inside the install prefix, e.g.: **$HOME/sr/share/sigrok-firmware** _(if you installed libsigrok into **$HOME/sr**)_
- In /usr/local: **/usr/local/share/sigrok-firmware**
- In /usr: **/usr/share/sigrok-firmware**
```
- [Source](https://www.sigrok.org/wiki/Firmware#Where_to_put_the_firmware_files)

after putting it into the right dir sigrok-cli detects it. Aswell as switching to the git version helped fix the problem of the driver not getting recognized. 

```shell
sigrok-cli --driver=kingst-la2016
```

selectes the driver. 
![[pulse-viewer-select.png]]
If the device does not show up unplug the device and plug it back in
