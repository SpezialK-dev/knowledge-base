# creating a PXE server
first to just boot a normal windows of this to install it. 

[a github guid](https://github.com/WillChamness/Dnsmasq-PXE)
[setting it up as tftp server](https://stelfox.net/blog/2013/12/using-dnsmasq-as-a-standalone-tftp-server/)
[A guid for getting windows over pxe boot](https://musteresel.github.io/posts/2018/04/install-windows-over-pxe-with-dnsmasq-pxelinux-winpe.html)

```shell
sudo dnsmasq  -d --interface=enp1s0 --dhcp-range=10.13.37.100,10.13.37.101,255.255.255.0,1h --dhcp-boot=bootx64.efi --enable-tftp --tftp-root=<path to your pxe root direcotry> --log-dhcp
```
though this command is not enought to get a working PXE boot. 
## Server Timout response 

[Unix stack exchange answer to this](https://unix.stackexchange.com/questions/635146/pxe-boot-error-pxe-e18-server-response-timeout)

it has to do with the some early PXE boot options having problems when the size is not specified 

add the following line to your config 
```shell
dhcp-option=option:boot-file-size,84
```
what value the size should be is soemthing you need to calculate by the following excert from the artikle  

> Some PXE boot clients (especially early versions of UEFI PXE boot clients) may require that the DHCP answer that identifies the boot file and the TFTP server to load it from, must also contain an option that indicates the size of the boot file (DHCP option #13: boot file size as a 16-bit unsigned value, units of 512-byte blocks, partial blocks rounded up to the next higher integer value).

> As your pxelinux.0 is sized 42694 bytes (= 83.3 blocks)

## Firewall

configure your firewall in a way where you can use that specific port. 
I will not go into that here sicne that is a different thing and is different from firewall to firewall

## dnsmasq: failed to create listening socket for port 53: Address already in use

this is usually the case because of systemd-resolved you can simply stop this service and everything will continue to work.

# Setting up for direct SSH connections 

because sometimes you might just want to directly connect through something 
[A unix stackexchange post about it ](https://unix.stackexchange.com/questions/295238/how-to-connect-to-device-via-ssh-over-direct-ethernet-connection)

the following bash script will set up an appropriate server.

```shell
sudo dnsmasq -d -C /dev/null --port=0 --domain=localdomain --interface=<your ethernet interface> --dhcp-range=192.168.9.2,192.168.9.10,99h
```

-C /dev/null means it does not read the normal config file 
--port=0 --domain=localdomain deaktivates the dns server 
