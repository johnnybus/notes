# MySQL Tunning
#### innodb_buffer_pool_size
This is resposible for memory area where InnoDB caches table and index data, when a larger buffer is set less disk I/O to access the same table data more than once.
The values to set are between 128 MB (default) and 8446744073709551615 (264-1) for 64-bit systems. Usually this value can be set up to 80% of our physical memory.
_"Competition for physical memory can cause paging in the operating system."_

#### innodb_log_file_size
This is reponsible for setting the redo logs size, redos logs register every transaction executed on the data. Redo can be used when recovery an database.
_"Large redo logs for good performance and small redo logs for fast crash recovery. If you know your application is write-intensive and you are using MySQL 5.6, you can start with innodb_log_file_size = 4G."_

#### max_connections
_"‘Too many connections’ error, max_connections is too low. It is very frequent that because the application does not close connections to the database correctly, you need much more than the default 151 connections. The main drawback of high values for max_connections (like 1000 or more) is that the server will become unresponsive if for any reason it has to run 1000 or more active transactions. "_

--
###### Useful url's:
- https://www.percona.com/blog/2014/01/28/10-mysql-performance-tuning-settings-after-installation/
- https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_buffer_pool_size
- http://www.tecmint.com/mysql-mariadb-performance-tuning-and-optimization/
