
# podman exists containers uppon ending ssh session

```
loginctl enable-linger $UID
```
simple issue simple fix, just run the above command that allows the service to run even after the users exited

# Rootless container cannot access files under SELinux

everyone recommends to use :z or :Z but for me just creating volumes worked way better, if you dont actually need to mount in something from the filesystem but just have storage. 
I dont understand why most people mount a filesystem path this seems like the more managable and better option especially if one later has things like kubernetes and such. 

# adding dockerhub to podman 

```shell
tee /etc/containers/registries.conf.d/010-dockerhub.conf <<'EOF' unqualified-search-registries = ["docker.io"] EOF
```