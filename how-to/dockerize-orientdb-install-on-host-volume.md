# Install Dockerized OrientDB with data storage on host volume

## Create directories on host

```
sudo mkdir /var/orientdb
sudo mkdir /var/orientdb/config
sudo mkdir /var/orientdb/databases
sudo mkdir /var/orientdb/backups
```


## Create container

Replace `password123` with a more secure password.

```
docker run -d --name orientdb -p 2424:2424 -p 2480:2480 \
    -v /var/orientdb/config:/orientdb/config \
    -v /var/orientdb/databases:/orientdb/databases \
    -v /var/orientdb/backups:/orientdb/backup \
    -e ORIENTDB_ROOT_PASSWORD=password123 \
    orientdb
```


## Run OrientDB console from container

```
docker run --rm -it orientdb /orientdb/bin/console.sh
```