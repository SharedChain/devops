# MariaDB Master-Master Replication

## Create 2 MariaDB Servers

### Secure each
```
sudo mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

Set root password? [Y/n] y
New password: 
Re-enter new password: 
Sorry, passwords do not match.

New password: 
Re-enter new password: 
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.
```

## Stop MariaDB on each

```
sudo systemctl stop mysql
```
## Edit configuration files

### Master Number 1

Edit /etc/mysql/my.cnf parameter file.

`sudo vi /etc/mysql/my.cnf`

Comment out `bind-address          = 127.0.0.1`

Add:
```
#
# * Replication
#
server-id               = 10
report_host             = master1
log_bin                 = /var/log/mysql/mariadb-bin
log_bin_index           = /var/log/mysql/mariadb-bin.index
relay_log               = /var/log/mysql/relay-bin
relay_log_index         = /var/log/mysql/relay-bin.index
auto_increment_increment = 5
auto_increment_offset = 1
```

### Master Number 2

Edit /etc/mysql/my.cnf parameter file.

`sudo vi /etc/mysql/my.cnf`

Comment out `bind-address          = 127.0.0.1`

Add:
```
#
# * Replication
#
server-id               = 20
report_host             = master2
log_bin                 = /var/log/mysql/mariadb-bin
log_bin_index           = /var/log/mysql/mariadb-bin.index
relay_log               = /var/log/mysql/relay-bin
relay_log_index         = /var/log/mysql/relay-bin.index
auto_increment_increment = 5
auto_increment_offset = 2
```

## Add Users and Permissions

### Master Number 1

#### Start MariaDB

```sudo systemctl start mysql```

### Login to MariaDB as root user

```sudo mysql -uroot -p```

#### Create replication user

```create user 'dbreplication'@'%' identified by 'PASSWD';```

#### Grant slave permissions to replication user

```grant replication slave on *.* to 'dbreplication'@'%';```

#### Flush privileges

```flush privileges```
