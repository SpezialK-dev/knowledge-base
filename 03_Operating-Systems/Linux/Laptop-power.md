# Laptop power saving 

if you want to enable power saving / limit the charging level one could use [TLP](https://wiki.archlinux.org/title/TLP).

simply install the package and enable the service. Configuration should be done for the User and is documented in the Arch wiki that is linked at the Top.

One way would be to create a ` /etc/tlp.d/01-battery.conf`
that contains the following content do set the max and minimum charge procennt that need to be meet for the laptop to charge. 
## Under thinkpads 

if you want to set a max charging level you should use the package [Treshy](https://aur.archlinux.org/packages/threshy)
to get this to work you need you should also install the [gui package](https://aur.archlinux.org/packages/threshy-gui)
