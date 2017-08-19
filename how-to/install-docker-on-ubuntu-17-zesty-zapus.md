# Install Docker on Ubuntu 17.04 (ZestyZapus)

We use Docker to both simplify deploys and maximize the resources on older, redeployed servers.

## Installation steps:
```
sudo apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu zesty stable"
sudo apt-get update
sudo apt-get install docker-ce
```
## Add users to docker group
```
 sudo adduser $USER docker
```

## Test Docker installation on Ubuntu 17.04 (ZestyZapus)
```
docker run hello-world
```
