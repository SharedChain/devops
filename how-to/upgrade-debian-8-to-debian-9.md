# Upgrade Debian 8 (Etch) to Debian 9 (Stretch)

There are times a service provider an only provision a machine with an older OS version. This is especially true when using older inexpensive dedicated hosts that have been redeployed.

These instructions only cover updating Debian 8 to 9 on a new server. 
If applications and services have already been installed you should follow a more [complete upgrade process.](https://linuxconfig.org/how-to-upgrade-debian-8-jessie-to-debian-9-stretch)

## Steps to Upgrade:
```
sudo sed -i 's/jessie/stretch/g' /etc/apt/sources.list
apt-get update
apt-get dist-upgrade
apt-get autoremove
```