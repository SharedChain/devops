# Upgrade Debian 8 (Etch) Sid (Unstable)

We need to run Sid/unstable to for cutting edge apps like Kubernetes.

## Steps to Upgrade:
```
sudo sed -i 's/stretch/sid/g' /etc/apt/sources.list
apt-get update
apt-get dist-upgrade
apt-get autoremove
```