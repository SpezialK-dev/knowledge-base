

# Disabling sleep button 

simply add 

```
HandleSuspendKey=ignore
```

to the `/etc/systemd/logind.conf` file to disable the sleep button 
- https://www.dotlinux.net/blog/how-to-handle-acpi-events-on-linux/#handling-acpi-events-with-systemd-logind
- https://github.com/systemd/systemd/issues/22936
