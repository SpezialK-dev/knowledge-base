
# limit battery charing using tlp
[TLP](https://wiki.archlinux.org/title/TLP)

1. install tlp as 

one should not have [power profiles](https://wiki.archlinux.org/title/CPU_frequency_scaling#power-profiles-daemon) active cause that creates issues

then one simply adds the following lines to a config under `/etc/tlp.d/XX-battery.conf`
```shell
# 01-battery.conf - limit charging
# See full explanation: https://linrunner.de/tlp/settings
START_CHARGE_THRESH_BAT0=30
STOP_CHARGE_THRESH_BAT0=80
```

(XX should be replaced with a valid number). that is enough to have power limiting. 

TLP can be used as a complete replacement for power profiles. 