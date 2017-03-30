Online MySQL/MariaDB migration to RDS
=====================================
This was tested with:
- MariaDB 10.1.16 -> RDS MariaDB 10.1.24
- MySQL 5.5 -> RDS Aurora 5.6
---

### Activate Bin Log and set Server ID on Master (you'll need to restart MySQL server)
```
[mysqld]
log-bin=mysql-bin
server-id=1
```
### Dump the database(s) with Master Data
```
mysqldump --databases <database_name> --master-data=2 --single-transaction --order-by-primary > database_backup.sql
```
### Remove any `DEFINER` from the dump
```
sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' database_backup.sql > database_backup_no_def.sql
```
### Import the dump into your RDS instance
```
mysql -h <rds_host> <database_name> < database_backup_no_def.sql
```
### Set the Replication Master on your RDS
```
CALL mysql.rds_set_external_master ('<master_address>', 3306, '<user>', '<pwd>', '<binary_log_file>', <position>, 0);
```
### Start the Replication
```
CALL mysql.rds_start_replication;
```
### Check the replication status
```
show slave status \G
```
### Test the creation of a table on the master and check if it was replicated
```
CREATE TABLE Persons (
    PersonID int,
    LastName varchar(255),
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255) 
);
```
### Check the replication seconds behind master, to ensure no transaction is missing
```
show slave status \G
```
### On your RDS slave, stop the replication and reset master info
```
CALL mysql.rds_stop_replication;
CALL mysql.rds_reset_external_master;
```
---
###### Source
- http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/MySQL.Procedural.Importing.NonRDSRepl.html