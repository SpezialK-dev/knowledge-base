
# Toolchain setup

[Standart Linux setup](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/get-started/linux-macos-setup.html)

[How to uninstall](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-guides/tools/idf-tools.html#idf-tools-uninstall)
# General Setup of an ESP Projekt 

everytime you use it you should run the cmd described below

```shell
idf.py set-target esp32
```

> The following command can be skipped in some cases 
```sh
idf.py menuconfig
```

> the following command actually builds projekt and everything
```Shell
idf build 
```


# Changes to your bashrc 

Id adviese that you you add a line to your bashrc

with esp-idf being the folder that you downloaded in the beginning of the documentation
```shell
alias get_idf=". $HOME/../../../esp-idf/export.sh"
```

this alias should be executed every time you are in a folder that has a project for an esp32 so that you can actually use the cmds as they are provided above. The alias is not important nor the location.