# Install LXD on Ubuntu 17.04 (ZestyZapus)

We use Docker to both simplify deploys and maximize the resources on older, redeployed servers.

## Installation steps:
```
sudo apt-get update
sudo apt-get lxd
sudo usermod -a -G lxd $USER
sudo lxd init

```

## LXD Initilization
```
sudo lxd init
```

using these options
```
Do you want to configure a new storage pool (yes/no) [default=yes]? 
Name of the new storage pool [default=default]: 
Name of the storage backend to use (dir, lvm, zfs) [default=zfs]: 
Create a new ZFS pool (yes/no) [default=yes]? 
Would you like to use an existing block device (yes/no) [default=no]? 
Size in GB of the new loop device (1GB minimum) [default=87GB]: 20
Would you like LXD to be available over the network (yes/no) [default=no]? 
Would you like stale cached images to be updated automatically (yes/no) [default=yes]? 
Would you like to create a new network bridge (yes/no) [default=yes]? 
What should the new bridge be called [default=lxdbr0]? 
What IPv4 address should be used (CIDR subnet notation, “auto” or “none”) [default=auto]? 
What IPv6 address should be used (CIDR subnet notation, “auto” or “none”) [default=auto]? none
LXD has been successfully configured.
```

## Test LXD installation on Ubuntu 17.04 (ZestyZapus)
```
lxc launch ubuntu:xenial x1
lxc list
lxc info x1
```