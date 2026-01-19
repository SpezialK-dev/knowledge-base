
I encountered a similar problem with wifi beeing bad on some networks (not all)

since the solution was recommending some network manager specific 
the solution is in the iwd.config man page.

To then set this config you would need to add the following lines to your config (the config can be found under `/etc/iwd/main.conf`)

```
[General]
AddressRandomization=disabled
```

based on this man page
```
├────────────────────────────────────────────────┼─────────────────────────────────────────────────────────────────┤
│ AddressRandomization                           │ Values: disabled, once, network                                 │
│                                                │                                                                 │
│                                                │ If AddressRandomization is set to disabled, the default  kernel │
│                                                │ behavior  is used.  This means the kernel will assign a mac ad‐ │
│                                                │ dress from the permanent mac  address  range  provided  by  the │
│                                                │ hardware  /  driver.  Thus it is possible for networks to track │
│                                                │ the user by the mac address which is permanent.                 │
│                                                │                                                                 │
│                                                │ If AddressRandomization is set to once, MAC address is  random‐ │
│                                                │ ized  a single time when iwd starts or when the hardware is de‐ │
│                                                │ tected for the first time (due to hotplug, etc.)                │
│                                                │                                                                 │
│                                                │ If AddressRandomization is set to network, the MAC  address  is │
│                                                │ randomized  on  each connection to a network. The MAC is gener‐ │
│                                                │ ated based on the SSID and permanent address  of  the  adapter. │
│                                                │ This  allows  the same MAC to be generated each time connecting │
│                                                │ to a given SSID while still hiding the permanent address.       │
├────────────────────────────────────────────────┼─────────────────────────────────────────────────────────────────┤
```

https://forums.linuxmint.com/viewtopic.php?t=443611
https://bbs.archlinux.org/viewtopic.php?id=309941
