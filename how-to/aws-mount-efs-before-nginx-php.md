# Mount an EFS share before starting Nginx/PHP/Node
This is required when you are mounting the configs or source files on an EFS share.

## Install NFS
`sudo apt-get install nfs-common`

## Install Amazon EFS Utilities
`sudo apt-get install amazon-efs-utils`

## Make a temp directory to mount the EFS share 
`sudo mkdir -p /mnt/efs/inbound-proxy-config`
`sudo mkdir -p /mnt/efs/node-files`
`sudo mkdir -p /mnt/efs/php-files`

## Temporarily mount EFS under a different path
`sudo mount -t efs fs-123abc456:/ /mnt/efs/inbound-proxy-configx`

## Copy the Nginx config files to the EFS share
`sudo cp -aR /etc/nginx/* /mnt/efs/inbound-proxy-config`

## Save a local copy of the Nginx configs
`sudo mv /etc/nginx /etc/nginx-orig`

## Create the final mount point Nginx expects to finds
`sudo mkdir /etc/nginx`

## Mount the EFS share at the final mount point
`sudo mount -t efs fs-123abc45:/ /etc/nginx`

## Ensure EFS gets mounted on reboot.
`sudo vi /etc/fstab`

Add:
```
fs-123abc456:/ /etc/nginx efs defaults,_netdev 0 0
```

## Find that mount point via sysyemctl
`systemctl list-units --type=mount`

Output will look like:
```
UNIT                         LOAD   ACTIVE SUB     DESCRIPTION
-.mount                      loaded active mounted /
dev-hugepages.mount          loaded active mounted Huge Pages File System
dev-mqueue.mount             loaded active mounted POSIX Message Queue File Syst
etc-nginx.mount              loaded active mounted /etc/nginx
run-rpc_pipefs.mount         loaded active mounted RPC Pipe File System
run-user-1000.mount          loaded active mounted /run/user/1000
sys-fs-fuse-connections.mount loaded active mounted FUSE Control File System
sys-kernel-debug.mount       loaded active mounted Debug File System
var-lib-lxcfs.mount          loaded active mounted /var/lib/lxcfs
```
It's this one: 
`etc-nginx.mount              loaded active mounted /etc/nginx`
    
To force Nginx to start *after* the mount we edit the Nginx systemd script.

Edit the Nginx `.service` file
`sudo systemctl edit --full nginx.service`

Add `etc-nginx.mount` to the *After*, to force loading after the /etc/nginx mount.
```
[Unit]
Description=A high performance web server and a reverse proxy server
After=network.target etc-nginx.mount
```
