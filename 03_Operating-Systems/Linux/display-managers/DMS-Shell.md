
- [matugen](https://archlinux.org/packages/extra/x86_64/matugen/ "View package details for matugen")  is needed for colors to be customized

# Under niri

```
 // Add to the end of ~/.config/niri/config.kdl
 include "dms/colors.kdl"
 include "dms/layout.kdl"
 include "dms/alttab.kdl"
 include "dms/binds.kdl"
```
needs to bee at the end of the config 


# Installing the greeter makes you modify the config file 
your unmodified file might look like this 
```
[terminal]
# The VT to run the greeter on. Can be "next", "current" or a number
# designating the VT.
vt = 1

# The default session, also known as the greeter.
[default_session]

# `agreety` is the bundled agetty/login-lookalike. You can replace `/bin/sh`
# with whatever you want started, such as `sway`.
command = "agreety --cmd /bin/sh"

# The user to run the command as. The privileges this user must have depends
# on the greeter. A graphical greeter may for example require the user to be
# in the `video` group.
user = "greeter"
```


the changes needed to this file are described in this file https://danklinux.com/docs/dankgreeter/installation




