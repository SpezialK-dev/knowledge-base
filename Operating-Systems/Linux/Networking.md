
# creating your own Setup

[a good overview of Software](https://wiki.archlinux.org/title/Network_configuration#Network_managers)
This gives a brief Overview

In my case I use IWD as DHCP client aswell as wifi client since it has a great shell interface for that. You need to [edit the config file ](https://wiki.archlinux.org/title/Iwd#Enable_built-in_network_configuration ). And also enable systemd-resolvd or resolvconf. whatever you choose. 


Also disable any other things like systemd networking aswell as dhcpcd and Network manager. 
if you want to use iwd aswell your '/etc/iwd/main.conf ' should look something similar to 
```shell
[General]
EnableNetworkConfiguration=true
[Network]
NameResolvingService=systemd
```
if you are using systemd for name resolution and iwd for everything else. 
This works so far. If you dont have these 4 lines it will not work and you will not be able to ping anything but it appears as if you are connected to a network but everything is unreachable. 

You could still get some problems if you are in some weird hotel wifis where you are directly between 2 accesspoints-> this creates might create problems. But I have not done enought research into that yet.

## using systemd-networkd
if you also want systemd-networkd you should start that service create configurations file following this [tutorial](https://wiki.archlinux.org/title/Systemd-networkd). But you need to disable the DHCP server from iwd. 
so Remove the following line from the above mentioned config
```shell
[General]
EnableNetworkConfiguration=true
```
and add the following lines to /etc/systemd/networkd.conf

```shell
DHCP=true
DHCPServer=true
```

If you follow the tutorial you should not includ the line 

```shell
IgnoreCarrierLoss=3s
```
if you are knowen to stay in weird places where things are not configured correctly since this might lead to DHCP not getting a new lease when switching access points. Which if they are incorrectly configured leads to problems.

the other lines in  /network/25-wireless.network files should look similar to this 

```shell
[Match]
Name=<name of your wlanconfig (example wlan0)>

[Network]
DHCP=yes #if you want to use the dhcp server

[DHCPv4]
RouterMetric=600

[IPv6AcceptRa]
RouterMetric=600
```



# PowerSaving 

Common trouble shooting things 
## Common symptoms

- Wifi randomly disconnecting 
- WIFI not connecting back after disconnect
- Dhcpcd deamon spewing random nonesense

## Solution
most often you just have multible services fighting for the same resources

1. check if you have multible networking things running if so disable the ones you dont want to use in my case I wanted to use iwd.
2. check for powre saving mode this miight improve your disconnects. Taken from here [arch wiki](https://bbs.archlinux.org/viewtopic.php?id=196375)


the following command disbles the powersaving of the wifi chip this might increase power usage but can get better stability in some certain situations.
```shell
sudo iw dev wlan0 set power_save off
```
This allows a quicker reconnect after a disconnect. But and can sometimes help but Otherwiese it does not really help if you have more services running which is usually the problem Check [creating your own Setup](# creating your own Setup) for a grafik of what to use with what. 
an alternative is to use iwconfig : 

```shell
sudo iwconfig wlan0 power off
```

### Persistance under IWD

under iwd you will need to add 
this option was found in the iwd.config man page!
```
[DriverQuirks]
PowerSaveDisable=*
```
Source : https://wiki.archlinux.org/title/Power_management#Network_interfaces
the driver name can be found via `lspci -k`

persistence can be checked via:
(this can be checked by simply calling `iwconfig` in a terminal)

then the following path might offer a solution 
https://askubuntu.com/questions/85214/how-can-i-prevent-iwconfig-power-management-from-being-turned-on

# Problem with wifi being down
[in my case working solution](## trying out iwctl and iw)

> the wlan0 adapter is mode DOWN or Dormant

wpa_cli : could not connect to wpa_supplicant  : wlan0

```shell
ip link set wlan0 up
```

does not set the wifi to up.
rfkill shows that it is unblocked


[The next tried solution](https://unix.stackexchange.com/questions/90778/how-to-bring-up-a-wi-fi-interface-from-a-command-line)

so I exectued the following.

```shell
ifconfig wlan0
```

curiosly enought this scans something
```shell
dhclient -v wlan0
```

This returns a 

```
DHCPDISCOVERY on wlan0 to 255.255.255.255 oirt 67 Interval <random numer between 3 and 20>
...
```

response 

```shell
iwlist wlan0 scan
```

also returns valid Wifi networks in my area? But wpa_cli does not work and the Interface is still DOWN as far as `ip link show` is concerned.

So I wonder if I can now connect with iwlist

## connecting to wifi with iwlist

since wpa_cli does not work we need to use a workaround.

This is a [Tutorial ](https://unix.stackexchange.com/questions/92799/connecting-to-wifi-network-through-command-line) on how to connect to that

we choose Option 2 since the other file just simply didnt exist on my current device.

ifconfig -> simply is a none existant command lol ???? (under live arch boot)
so we are skipping that step

```shell
# iwlist wlan0 scan
```
this should return a bunch of different WiFi networks choose the one you want to connect to 

[Arch wiki for iwconfig](https://wiki.archlinux.org/title/Network_configuration/Wireless#Utilities)

```shell
iwconfig wlan0 essid <name of Hotspot> key s:<password>
```

This returns in my case 
```Error
Error for wireless request "set Encode (8B2A)"
	SET failed on device wlan0 ; invalid argument
```

Searching this problem it returns 2 search results 
[arch wiki](https://bbs.archlinux.org/viewtopic.php?id=72898)
[this weird wordpress PDF](https://curmaraca.wordpress.com/wp-content/uploads/2015/09/error-for-wireless-request-set-encode-8b2a-wep.pdf)

since none of these returned anything sensible, my next choice of action was converting the password to hex via [CyberChef](https://cyberchef.org/#recipe=To_Hex('Space',0))

```shell
iwconfig wlan0 essid <name of Hotspot> key <password in hex>
```
this also for some reason did not work 

## trying out iwctl and iw 

after opening the iwctl 
```shell
iwctl 
```



the following commands all will be executed in the iwctl shell


```iwctl
> adapter list
<shows a list of adapters not usefull for what we want>
> station list
<actually shows the status of our adapter wlan0 as disconnected>
> station wlan0 scan
> station wlan0 get-networks
> station wlan0 connect SSID 
```

[man pages for iwctl](https://man.archlinux.org/man/iwctl.1) -> also contains a guid on how to connect 
THIS ACtually worked and the adapter was now up


## some other usefull commands in the shell


```ìwctl
device list 
<shows you the device name what adaper its on and what mode its in>
```
# wpa_supplicant

[Arch linux docs](https://wiki.archlinux.org/title/Wpa_supplicant)
uses the wpa_cli to actually do all of the connecting things

## connecting to wifi network
based mostly on this [forum thread](https://sirlagz.net/2012/08/27/how-to-use-wpa_cli-to-connect-to-a-wireless-network/) with some things more flashed out and hopefully better explained

$ = shell commands 
\> = commands inside wpa_cli
\# = comments and should be ignored

\wlp* means any network interface that starts with wlp 
```shell
#getting the right wifi access point
$ ip link show
<list of host addresses>
<one should be named wlp*>

#invoking wpa_cli
$ wpa_cli -i <interfaceName (wlp*)>
Selected interface 'wlan0'

Interactive mode
> scan 
<2>CTRL-EVENT-SCAN-RESULTS

>scan_results
#example data taken from forum thread
bssid / frequency / signal level / flags / ssid
00:55:ab:25:ac:5a 2462 -71 [WPA2-PSK-CCMP][ESS] WLAN-Network
00:11:99:51:ba:f0 2437 -64 [WPA2-PSK-CCMP][ESS] WLAN-Network2
> add_network #creates a new network
0 #network ID
> set_network 0 ssid "WLAN-Network" #replace your Wlan network with said ssid 
> set_network 0 psk "<insert the password for said network>"
> enable_network 0
> save_config # not yet tested 
> reconnect # not really needed 
```

for more info go to the above linke forum thread, this is more of a small rundown of how to use it 


## adding it a bit more permanently
we use the 
```sh
> save_config
```
command


# Systemd-Network
[Arch wiki](https://wiki.archlinux.org/title/Systemd-networkd)

## Problems where I dont get a connection after some time

1. I checked I didnt have another DHCP server running 
2. I identified, that Systemd-networkd was the problem since restarting this fixed me not getting anything 
3. figured out that the problem was most likely similar to this [Github Issue ](https://github.com/systemd/systemd/issues/13432)

It seems to be a problem with resolved
it appears to be a catch all error so you need to figure it out, look at resolved with --verbose or debug flag

also setting
```config
DNSSEC=no 
```
in the config file and then restarting resolved the issue for some peoeple as choosen by [this stackoverflow post](https://superuser.com/questions/1676584/using-degraded-feature-set-tcp-instead-of-udp-for-dns-server)

To enable debuging follow this  [issue](https://github.com/systemd/systemd/issues/17330)
to see your current dns server you can run 

```shell
sudo resolvedctl
```


# Forwarding traffic from another devices over wifi

this is a small guid to turn to forward traffic that you recieve from your ethernet port to your wifi connection. This allows you to connect another pc to your device and give that pc a ethernet connection. Even though you only have wifi. 


[Superuser post](https://superuser.com/questions/684275/how-to-forward-packets-between-two-interfaces) has some start which might work. 


# IWD

## Setting up developer mode 

[Forum Post](https://unix.stackexchange.com/questions/738561/how-can-i-enable-iwd-debug-mode)

```shell
sudo /usr/lib/iwd/iwd -E
```

you should stop the systemd service before doing this. When this was done 
the systemd service is under the following directory
/usr/lib/systemd/system/iwd.service 

you might want to edit that file if you want to permanently have access to debug features which could be handy in some situations for example being able to force certain things

## Setting up things like 8021x Networks

[ARch wiki](https://wiki.archlinux.org/title/Iwd#EAP-PEAP)

In this case we need to create a file in /var/lib/iwd/nameOfnetwork.8021x

in this case it was a EAP-PWD network so we just copyied the config from the arch wiki. This should allow you to then autoconnect to the network.

## EAD

The wired driver for eathernet devices 


to run it in debug mode

```shell

sudo /lib/iwd/ead -d
```


# IW 
[Arch wiki page](https://wiki.archlinux.org/title/Network_configuration/Wireless#Utilities)
also allows you to manage networks without security aswell as some very low level things. 
like controlling frequencies. 

in Khz. This apears to allow Very low level actions 

# some testing things

see all open ports helpfull when debugging server applications 
```shell
ss -tunlp
```
## tcpdump

you should have this installed

```shell
$tcpdump -D
``` 
gives you all interfaces and everything that you can listen to with tcpdump

with 
```shell
tcpdump -i <interface>
```
you will then listen to that specific interface 

# Firewalls

under arch if you want to use nftables as your firewall instrall the 
iptables-nft package since base and some other packages still depend on iptables even though you dont want to use it 
