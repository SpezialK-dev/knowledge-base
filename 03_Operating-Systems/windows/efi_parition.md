# opening the efi partition on running device 

to open the windows parition on a already booted windows 
the following commands archieve mounting the efi partiion 

```windows shell
diskpart
list disk 
select disk 0
list partition 
select partition 1
assign letter=b
exit
```
then you might be able to open it if you run explorer as admin 
this could be archived through the following command: 
```windows shell

taskkill /im explorer.exe /f
explorer.exe
```
though this didnt work for me so I opted to go the following route
The only way I managed to browse then was using the command prompt. 
and using 
```windows shell
B:
```
and then browsing it in the termina if you want to do it you should probably do it in the windows recoveryshell 
