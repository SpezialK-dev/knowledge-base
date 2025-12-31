# Knowen problems 
Sway currently has problems with lightDM -> if you use LightDM it just will not start. 

# Some pluggins 

[wiki](https://github.com/swaywm/sway/wiki/Useful-add-ons-for-sway)


# Java Gui applications 

if you have java applications that are writen with the normal java application framework, you might encounter the application starting but the thing just staying white. 
In that case add the following line to your bashrc and exit sway and reload the bash config. 
```bash
export _JAVA_AWT_WM_NONREPARENTING=1
```

After that the application should no longer stay white 

# Setting Brightness 

```config 
bindsym XF86MonBrightnessUp exec brightnessctl set 10%+
bindsym XF86MonBrightnessDown exec brightnessctl set 10%-
```


# launchers 

https://github.com/Cloudef/bemenu -> if you want the most similar to i3 experience 


# Launching things on specifc 

[readme](https://gist.github.com/3lpsy/9fc13dae3ba9c176013e3f6457b458e2)




# XDG-Desktop missing implementation 

in Zed

https://github.com/zed-industries/zed/issues/14874 might have some answers to this problem 

for sway this should be fixed with the `xdg-desktop-portal-gtk` package under arch 

as can be taken from this table https://wiki.archlinux.org/title/XDG_Desktop_Portal#List_of_backends_and_interfaces


# Screenflickering in some applications 

like zed or some other apps after 1.11  version on sway

https://github.com/swaywm/sway/issues/8755

add the following line to your bashrc
```bash
export WLR_RENDER_NO_EXPLICIT_SYNC=1
```


# Adding gammastep support 

https://wiki.gentoo.org/wiki/Gammastep

some weird behavior I noticed, was that I could not toggle the red light via the in build gtk icon on my swaybar, if I was starting gammastep via the sway config 
ie 
```conf
exec gammastep
```

it would do its job but you could not control it. 

the correct way to do it was to enable the gammastep service 
```shell
systemctl --user enable gammastep.service
systemctl --user start gammastep.service
```

then the icon in swaybar also worked. Fully reading wiki pages sometimes helps. 