---
tags:
  - Troubleshooting
  - Bootloaders
  - Linux
---
# Problem 


Nothing loads after booting up your linux ? 
(does not fix all problems )
# Troubleshooting 

The generall steps to troubleshoot are :
1. try and get into intramfs -> can fix some stuff 
2. after that good luck. 

If not even an initramfs loads check  if `quite` is set in your linux kernel params if so remove it. 

To load into the Initramfs just follow the following steps : 


> To make the boot process stop in the `initramfs`, you have to add `break` to the kernel line in `grub`.  
 > There are different stages and one can instruct to stop at a given stage. The default is `premount`, for further options and a comprehensive description refer to [man initramfs-tools](http://manpages.ubuntu.com/manpages/trusty/man8/initramfs-tools.8.html).
>
> It is often a hassle to get the `grub` menu during boot, but normally Esc will raise it, when pressed at the right time. Maybe pressing it again and again during boot will raise your chance.  
 > Once in the boot menu of `grub`, select the boot entry you want to boot with your Up/Down keys and press e to edit the entry.  
> Then navigate down to the line that starts with `linux` and append `break` or `break=<run-time>` to make the boot process stop in the `initramfs`.
- From [AskUbunut](https://askubuntu.com/questions/1043242/how-do-i-force-ubuntu-to-boot-into-initramfs)

meaning you just write a break into your linux kernel parameters (denoted by the `linux` at the start of the line in grub and in systemdboot its the only thing you can edit)

([Linux kernel parameters](https://www.kernel.org/doc/html/latest/admin-guide/kernel-parameters.html) if needed )
# Solution

It is possible your system is corrupted and you can repair it using fsck.
Simply type `exit` in the initramfs  and it should tell you what the actual problem is. Since that will exit the initramfs and start booting normaly

```shell
fsck /dev/<drive>
```

(optionally -p can be used to automatically repair things that can be savely repaird)

This will repair filesystem and fix some issues is not a magic solution to everything. 

