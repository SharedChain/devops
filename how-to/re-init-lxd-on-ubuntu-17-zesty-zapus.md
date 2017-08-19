# Install LXD on Ubuntu 17.04 (ZestyZapus)

We use Docker to both simplify deploys and maximize the resources on older, redeployed servers.

## Installation steps:
```
sudo apt-get update
sudo apt-get lxd
sudo usermod -a -G lxd
sudo lxd init

```


## Test Docker installation on Ubuntu 17.04 (ZestyZapus)
```
docker run hello-world
```