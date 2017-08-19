# Install Portainer on Ubuntu 17.04 (ZestyZapus)

## Requirements:

1. Running Docker enviro
2. Working network connection

## Steps to install Portainer

### Create a password

We use a container for this and store the output. 
Change the `password123` to something more secure before running.

```
 PORTAINER_PWD="$(docker run --rm httpd:2.4-alpine htpasswd -nbB admin password123 | cut -d ":" -f 2)"
```

### Spin up the Portainer container

We use the stored password and create a volume mapping the local `docker.sock` to the same place on the container.
Reagrdless of where your `docker.sock` is you want to map it to `/var/run/docker.sock` on the Portainer container.


```
docker run --name portainer -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer --admin-password  $PORTAINER_PWD
```