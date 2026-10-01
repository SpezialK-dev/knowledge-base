# Reading memory under linux

## Sources 

- [Kernel documentation on Memory](https://www.kernel.org/doc/gorman/html/understand/understand005.html)
- [Blackhat Talk on Code injection via /dev/mem](https://www.blackhat.com/presentations/bh-europe-09/Lineberry/BlackHat-Europe-2009-Lineberry-code-injection-via-dev-mem.pdf)
- [Superuser.com bypassing 1MB limit](https://superuser.com/questions/1500615/how-can-i-overcome-1mb-restriction-of-dev-mem)


## Some aquired knowleadge 

- it lets you access all physical memory
- this also mapps some IO things, things like your GPU mem and other peripherals might also be in this 
- CONFIG\_STRICT\_DEVMEM makes the kernel check addresses in
- you can bypass it with a kernel modul like fmem
- The readable part is capped at 1MB you can check this yourself with the following command 


```shell
sudo cat /dev/mem | wc
```
your result should look similar to this 
```result 
    824    4850 1048576
```
only the last part is relevant it should be the same for you. [This information was taken from stackoverflow](https://stackoverflow.com/questions/6134984/access-permissions-of-dev-mem)

### Some new tooling 

Since /dev/mem has a limit you might want to either use 
- [Lime](https://github.com/504ensicslabs/lime)
- [fmem](https://github.com/NateBrune/fmem)
both are unmaintained and would allow you to do a similar thing. If you just want to read memory


## Actually doing it 


you could do it like this 

```shell
sudo hexdump -C --skip 0x0005000 /dev/mem | head
```

0x0100000(00f0000) apears to be the highest possible address we can read out with sudo privilages under a normal arch install. 
### working with Lime and fmem
[A forensics guid](https://forensecurity.blogspot.com/2013/12/linux-memory-forensics.html)


### with c/c++

you can open it with [open](https://www.man7.org/linux/man-pages/man2/open.2.html)
a guid [from eeces umich](https://www.eecs.umich.edu/courses/eecs380/HANDOUTS/cppBinaryFileIO-1.html)


in general it can be done like any other stream
```c++
//meaning the following code works
std::ifstream fd;

//since we have a binary data stream
fd.open(/dev/fmem, std::ios::in | std::ios::binary);

if(!fd){
	//error handdling
}
//len is an abitraray value
char* buffer = new char[len];

//this reads the file
//read data is writen into buffer, len is the amount that will be writen into buffer
fd.read(buffer, len);

global_pos += len
//moves the fd pointer forward
fd.seekg(global_pos)
```
