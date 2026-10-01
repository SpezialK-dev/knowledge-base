ZG5 the the one that I have 
They are absolut tiny 
# Hardware spec

- 1.5 GB DDR2 Ram (the [max amount](https://www.computer-specifications.com/specifications/Acer-AspireOneZG5-Specs.html) of Ram it can accept apparently)
- 1.6 GHz Single Core [Intel Atom](https://ark.intel.com/content/www/us/en/ark/products/36331/intel-atom-processor-n270-512k-cache-1-60-ghz-533-mhz-fsb.html) 

### IO 

it has decent IO for its size 

- 1 VGA 
- 1 Ethernet 
- 2 SD Card Readers (1 Marked as Storage expension)
- 3 USB Ports
- 1 Headphone Port
- 1 Microphone port
- 1 Kensingten Lock 
# The plan with this hardware

install [void linux](https://voidlinux.org/) on it since they still have a 32 bit support. install i3 ontop of the already existing xfce thing maybe ? or just a clean i3 install. 

# Problems Encountered 

## Wifi
if you have some problems with connecting wifi in general there are some firsts steps that are descriped in [here](https://old.reddit.com/r/voidlinux/comments/hjcyun/very_simple_guide_on_how_to_using_wpa_supplicant/) that are also as follows the rest should be done with the guid in for  [wpa_cli](Networking.md#wpa_supplicant). 

all of the following commands are void Linux specif and do not work under System D but only a runnit based system. 
```shell
#creates the Services for dhcpcd and wpa_supplicant
$ sudo ln -s /etc/sv/dhcpcd /var/service 
$ sudo ln -s /etc/sv/wpa_supplicant /var/service 
# starts up the services
$ sudo sv up dhcpcd
$ sudo sv up wpa_supplicant
$ ip link show

<A list of interfaces >
```
WIFI devices usually are something like wlp and eth devices are Ethernet interfaces. 