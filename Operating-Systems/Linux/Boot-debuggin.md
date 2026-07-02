A world filled with pain 

# Your Systemd Died for Mystical reasons here are some sources to help you debug your fuckup 

some general Links
[Linux kernel parameters](https://www.kernel.org/doc/html/latest/admin-guide/kernel-parameters.html)

## Getting into the Initramfs 


If this does not load well you are in a world of pain. 
So lets test if even the initramfs is existant and loads or if there is some other problem. 

based on the following answer  


> To make the boot process stop in the `initramfs`, you have to add `break` to the kernel line in `grub`.  
 > There are different stages and one can instruct to stop at a given stage. The default is `premount`, for further options and a comprehensive description refer to [man initramfs-tools](http://manpages.ubuntu.com/manpages/trusty/man8/initramfs-tools.8.html).
>
> It is often a hassle to get the `grub` menu during boot, but normally Esc will raise it, when pressed at the right time. Maybe pressing it again and again during boot will raise your chance.  
 > Once in the boot menu of `grub`, select the boot entry you want to boot with your Up/Down keys and press e to edit the entry.  
> Then navigate down to the line that starts with `linux` and append `break` or `break=<run-time>` to make the boot process stop in the `initramfs`.
- From [AskUbunut](https://askubuntu.com/questions/1043242/how-do-i-force-ubuntu-to-boot-into-initramfs)

meaning you just write a break into your linux kernel parameters (denoted by the `linux` at the start of the line in grub and in systemdboot its the only thing you can edit)



## Nothing loads after initramfs 

possibly your system is corrupted and you can repair it using fsck.
Simply type `exit` in the initramfs  and it should tell you what the actual problem is. Since that will exit the initramfs and start booting normaly

```shell
fsck /dev/<drive>
```

(optionally -p can be used to automatically repair things that can be savely repaird)

## No initramfs and nothing else shows up 

Check the following things :
- if `quite` is set in your linux kernel params if so remove it. 
- 