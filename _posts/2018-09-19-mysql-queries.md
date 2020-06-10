---
layout: post
title: MySQL SQL Queries
date: 2018-09-19
---  

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
# LOG all SQL queries
```sql
SET GLOBAL general_log = 'ON';
SET GLOBAL log_output = 'table';

select * from mysql.general_log;
```
