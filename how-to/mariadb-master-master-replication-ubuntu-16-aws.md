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

#### Edit /etc/mysql/my.cnf parameter file.

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
#### Edit /etc/hosts

`sudo vi /etc/hosts`

Add 
```10.0.0.10 master2```

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

#### Edit /etc/hosts

`sudo vi /etc/hosts`

Add 
```10.0.0.81 master1```

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

#### Check master1 status

```MariaDB [(none)]> show master status;
+--------------------+----------+--------------+------------------+
| File               | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+--------------------+----------+--------------+------------------+
| mariadb-bin.000003 |      329 |              |                  |
+--------------------+----------+--------------+------------------+
```

### Master Number 2

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

#### Start Replication

```
STOP SLAVE;
CHANGE MASTER TO MASTER_HOST='master1', MASTER_USER='dbreplication', MASTER_LOG_FILE='mariadb-bin.000003', MASTER_LOG_POS=329;
START SLAVE;
```
#### Verify things are syncing

```MariaDB [(none)]> show slave status\G
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: master1
                  Master_User: dbreplication
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mariadb-bin.000003
          Read_Master_Log_Pos: 753
               Relay_Log_File: relay-bin.000002
                Relay_Log_Pos: 963
        Relay_Master_Log_File: mariadb-bin.000003
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
           Replicate_Do_Table: 
       Replicate_Ignore_Table: 
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
                   Last_Errno: 0
                   Last_Error: 
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 753
              Relay_Log_Space: 1255
              Until_Condition: None
               Until_Log_File: 
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File: 
           Master_SSL_CA_Path: 
              Master_SSL_Cert: 
            Master_SSL_Cipher: 
               Master_SSL_Key: 
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error: 
               Last_SQL_Errno: 0
               Last_SQL_Error: 
  Replicate_Ignore_Server_Ids: 
             Master_Server_Id: 10
               Master_SSL_Crl: 
           Master_SSL_Crlpath: 
                   Using_Gtid: No
                  Gtid_IO_Pos: 
      Replicate_Do_Domain_Ids: 
  Replicate_Ignore_Domain_Ids: 
                Parallel_Mode: conservative

```
