# Useful LXD commands
Summarized from https://stgraber.org/2016/03/19/lxd-2-0-your-first-lxd-container-312/. 

Interestingly, the LXD command line client is named.... `lxc`! 

### List available containers
```
lxc image list ubuntu:        # ubuntu: is officially supported image source
lxc image list images:        # images: is an unsupported source
lxc image alias list images:  # lists user-friendly names
```
### Launch a container
This creates and starts a container.
```
lxc launch ubuntu:14.04 CONTAINERNAME   # image and container names are optional 
lxc launch ubuntu:14.04/armhf armcont   # specific architecture
lxc launch images:alpine/3.3/amd64      # unsupported images: source
```
#### Create container
Without starting it.
```
lxc init images:alpine/3.3/amd64 alpinecont
lxc copy CONTAINER1 CONTAINER2        # clone
lxc delete alpinecont [--force]       # --force if it is running
```
#### Start/stop after creating it
```
lxc start alpinecont
lxc stop alpinecont [--force]         # --force if it doesn't want to stop
lxc restart alpinecont [--force]
lxc pause alpinecont                  # SIGSTOP to all container processes
```
### List local containers
``` 
lxc list 
lxc list --columns "nsapt"            # name, status, arch, PID of init, type
lxc list security.privileged=true     # filter for properties
lxc info CONTAINERNAME                # detailed info about one container
```
#### Available columns for the list command
```
    4 - IPv4 address
    6 - IPv6 address
    a - Architecture
    c - Creation date
    n - Name
    p - PID of the container's init process
    P - Profiles
    s - State
    S - Number of snapshots
    t - Type (persistent or ephemeral)
```
### Rename
```
lxc move CONTAINERNAME NEWNAME
```
### Configuration
Config changes are  effective immediately, even if container is running.
```
export VISUAL=/usr/bin/vim
lxc config edit CONTAINERNAME           # launches editor
lxc config set CONTAINERNAME KEY VALUE  # change a single config item
lxc config device add CONTAINERNAME DEVICE TYPE KEY=VALUE
lxc config show [--expanded] CONTAINERNAME
Configuration settings can be saved as **profiles**.
```
### Enter the container
```
lxc exec CONTAINERNAME -- PROGRAM OPTIONS
lxc exec CONTAINERNAME sh
lxc exec CONATINERNAME --env KEY=VALUE PROGRAM   # environment variable
```
This command runs the program in all the namespaces and cgroups of the container. The program must exist inside the container. 
### Access container files
```
lxc file pull CONTAINERNAME/etc/passwd /tmp/mypasswd
lxc file push /tmp/mypasswd CONTAINERNAME/etc/passwd 
lxc file edit CONTAINERNAME/etc/passwd 
```
### Snapshots
```
lxc snapshot CONTAINERNAME SNAPNAME    # SNAPNAME is optional; default name snap*X*
lxc restore CONTAINERNAME SNAPNAME     # resets the container to snapshot
lxc copy CONTAINERNAME/SNAPNAME NEWCONTAINER               # new container from snapshot
lxc delete CONTAINERNAME/SNAPNAME
lxc info CONTAINERNAME                 # lists snapshots among other info
lxc move CONTAINERNAME/SNAPNAME CONTAINERNAME/NEWSNAPNAME  # rename snapshot
