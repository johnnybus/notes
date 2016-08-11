# Mysql Tips
## Common commands
###### Show all database
    show databases;
###### Create database
    CREATE DATABASE mydatabase;


## Manage Users and permissions
###### Create user
    CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
###### List all users
    select user from mysql.user group by user;
###### Grand basic permissions to work on a database
    GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, INDEX, ALTER, CREATE TEMPORARY TABLES ON mydatabse.* TO 'newuser'@'localhost';
###### Grant all permissions to user on specified database
    GRANT ALL PRIVILEGES ON mydatabse.* TO 'newuser'@'localhost';
###### Grant all permissions to user on all databases
    GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'localhost';
###### Flush privileges
    FLUSH PRIVILEGES;


## System Administrator Basics
##### Install MariaDB (a.k.a. MySQL)
    yum install mariadb mariadb-server
##### Enable, Disable MariaDB service
    systemctl enable mariadb
##### Start, Stop, Restart MariaDB service
    systemctl start mariadb
##### Short status message from the server
    mysqladmin status
##### Gives an extended status message from the server
    mysqladmin extended-status
##### Show list of active threads in server
    mysqladmin processlist
##### MySQL Wide Mode, allows anyone anywhere log on MySQL server
    mysqld_safe --skip-grant-tables &
##### Login into MySQL
    mysql -u root -p
##### Change user password
    UPDATE mysql.user SET Password=PASSWORD('put your password here') WHERE User='root';


## Backups and Restore
###### Backup a specific Database
    mysqldump -u root -p mydb >> mydb-`date +%F`.sql
###### Backup all databases
    mysqldump -u root -p --opt --all-databases >> alldbs-`date +%F`.sql
###### Restore Database
    mysql -u root -p < backup.sql
---
##### MYSQL dump using credentials file
###### Create a credentials file on your home directory (`~/.my.cnf`) like this:
    user=dbbackup
    password=secretofgods
###### Make sure that the permissions are set to `600` in order to ensure that only the owner can manage its content
    chmod ~/.my.cnf
###### The mysqldump command will look for the file `~/.my.cnf` and use it
    mysqldump mydb >> mydb-`date +%F`.sql


## Manage MariaDB configuration file
###### To change listining IP's, edit `/etc/my.cnf`, and above the tag `[mysqld]`, add the following line. In this case, mysql server will only accept connections from localhost.
    bind-address=127.0.0.1
###### Enable query logs, edit `/etc/my.cnf`, and above the tag `[mysqld]`, add the following line.
    general_log=1
    general_log_file=/home/dbuser/query.log
###### Restart MySQL to apply settings on configuration file, its required to reload or restart the server
    systemctl restart mariadb


---
###### Useful URL's
[Basic MySQL Administration](https://itconnect.uw.edu/connect/web-publishing/shared-hosting/using-mysql-on-shared-uw-hosting/basic-mysql-administration/)
[DigitalOcean MySQL Guide](https://www.digitalocean.com/community/tutorials/how-to-create-a-new-user-and-grant-permissions-in-mysql)
[Backup Tool Percona](https://www.percona.com/)
