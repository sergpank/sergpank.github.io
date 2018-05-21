---
layout: post
title: PostgreSQL CheatSheet
date: 2018-05-21
---  

# Install and use Postgresql9.6 from console

0. Links:
**YUM Installation**: https://wiki.postgresql.org/wiki/YUM_Installation
**First Steps**:      https://wiki.postgresql.org/wiki/First_steps

1. Install:
To list available packages:
`yum list postgresql*`
For example, to install a basic PostgreSQL 9.6 server:
`yum install postgresql96-server`

2. Data directory for database will be at:
`/var/lib/pgsql96/data`

3. Initialize service:
`service postgresql96 initdb`

4. Set PostgreSQL start automatically when OS starts:
`chkconfig postgresql96 on`

5. Control service
To control the database service, use:
`service <name> <command>`
where <command> can be:
```
start : start the database
stop : stop the database
restart : stop/start the database; used to read changes to core configuration files
reload : reload pg_hba.conf file while keeping database running
```
E.g. to start version 9.6:
`service postgresql-9.6 start`

6. Removing. To remove everything:
`yum erase postgresql96*`


# First steps after installation:
0. `sudo -s`     - swith to **root** user to be able to login as *postgres* user
1. `su postgres` - switch to **postgres** user for manipulations with DB.
2. `psql help`   - check that **psql** command works fine and PostgreSQL is really running.

`CREATE SCHEMA test;`
`CREATE USER ec2 PASSWORD 'jkl123';`
`GRANT ALL ON SCHEMA test TO ec2;`
`GRANT ALL ON ALL TABLES IN SCHEMA test TO ec2;`
`CREATE DATABASE kishinev OWNER postgres`

1. \du                       ---> List all users
2. drop user --echo ec2      ---> drop user `ec2`
3. createuser --no-superuser --no-createdb --no-createrole --echo --pwprompt ec2


# Change password:
1. psql
2. \password
3. enter new password twice
4. find 'pg_hba.conf' file and add/update the following line: (file is here -> /var/lib/pgsql96/data/pg_hba.conf)
>>> FROM >> `local   all             postgres                                peer`
>>> TO   >> `local   all             postgres                                md5`
5. restart *postgresql96* service:
>>> `sudo service postgresql96 restart`
6. Source ---> https://stackoverflow.com/questions/18664074/getting-error-peer-authentication-failed-for-user-postgres-when-trying-to-ge

# PSQL commands:
1. `psql -U postgres -d test` ---> Connect as user **postgres** to database **test**
2. \du                        ---> list all users
3. \l OR \list                ---> list all databases
4. \dt                        ---> list all tables

# Dump Database:
`pg_dump -U postgres kishinev > kishinev.sql`
will create a SQL sump of database **kishinev** using user **postgres**

# Restore Database Dump:
`psql -U postgres`
`CREATE DATABASE kishinev OWNER postgres;`

`psql -U postgres kishinev < kishinev.sql`