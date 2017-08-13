# Initialize Debian basic build

This should be done as the first step on a new build.

## Steps:
```
su
apt-get update
apt-get dist-upgrade
apt-get install sudo
usermod -aG sudo <username>
```

The <username> should match the user entered during the Debian install.