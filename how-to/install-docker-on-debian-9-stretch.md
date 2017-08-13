# Install Docker on Debian 9 (Stretch)

We use Docker to both simplify deploys and maximize the resources on older, redeployed servers.

## Installation steps:
```
sudo apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian stretch stable"
sudo apt-get update
sudo apt-get install docker-ce
```

## Test Docker installation on Debian 9 (Stretch)
```
sudo docker run hello-world
```