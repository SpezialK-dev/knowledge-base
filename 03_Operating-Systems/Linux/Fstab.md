[Arch wiki](https://wiki.archlinux.org/title/Fstab)


IF you want to modify your /etc/fstab config manually (cause why the fuck not )

you should always run 

``` shell
sudo mount -a 
```

if this runs without any errors it is safe to reboot your system, IF errors occur FIX them, immediately. Since if those are not fixed your system will refuse to boot. 


# Some Trouble shooting things

## if you cannot execute programms
if you cannot execute programms on said drive add the exec option to said drive in the fstab config 

## if the folder is owned by root 
add the uid=1000 (usually otherwise  get with 

```sh
echo $UID
```
)
and gid=1000 to the options of the fstab config