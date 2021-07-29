---
layout: post
title: MySQL Cheatsheet (commands and queries)
date: 2018-09-19
---  

## Basic console commands:
```bash
# connect to some db:
mysql -h hostname -u username -pPassword schemaname

# create dump of the database (DB - test; dump - test.dump; host - localhost):
mysqldump -u root test > test.dump

# restore dump of the database:
mysql> create database test;
mysql> use test;
mysql> select database();
mysql> source test.dump

# create dump of the table:
mysqldump -u username -pPassword -h hostname databasename tablename > dumpname

# restore dump of the table:
mysql -h hostname -u username -pPassword databasename < dumpname

# connect to local DB as root:
mysql -uroot

# list available schemas:
show databases;

# select schema
use schema-name;

# show current schema:
select database();
```

## Show size for all DB tables:
```sql
SELECT 
     table_schema as `Database`, 
     table_name AS `Table`, 
     round(((data_length + index_length) / 1024 / 1024), 2) `Size in MB` 
FROM information_schema.TABLES 
ORDER BY (data_length + index_length) DESC;
```
## Show all table indexes:
```sql
SELECT DISTINCT
    TABLE_NAME,
    INDEX_NAME
FROM INFORMATION_SCHEMA.STATISTICS
where table_name='opx_subscriptions';
```
## Show DB Timezone
```sql
-- choose query you like
SELECT @@session.time_zone;
SELECT @@system_time_zone;
SELECT IF(@@session.time_zone = 'SYSTEM', @@system_time_zone, @@session.time_zone);
```
## LOG all SQL queries
```sql
SET GLOBAL general_log = 'ON';
SET GLOBAL log_output = 'table';

select * from mysql.general_log;
```
## Count connections from all hosts
```sql
select substr(host, 1, locate(':', host) - 1) as ip, count(*) as connections_nr
from information_schema.processlist
group by ip
order by connections_nr desc;

-- for quick view
show processlist;
```
