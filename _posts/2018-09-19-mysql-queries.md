---
layout: post
title: MySQL SQL Queries
date: 2018-09-19
---  

## Show size for all DB tables:
```
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
