

# Missing icons in some GTK applications 

in some cases there are missing icons in GTK applications this especially happens if you are using a window manager like niri / i3 or similar to fix this simply add the following command to your startup script / config 

```shell
gsettings set org.gnome.desktop.interface icon-theme Adwaita
```
The icon theme can be replaced with what you want to use. 

As taken from [Manjaro Forum](https://forum.manjaro.org/t/missing-gtk-4-icons-with-fix/124075)
