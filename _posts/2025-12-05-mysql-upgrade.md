---
layout: post
title: Upgrade MySQL from 5.x to 8.x with Homebrew
date: 2025-12-05
categories: mysql homebrew bash mac
---

1. [Optinal] Backup Your Databases:
```bash
# better make backup only for necessary databases, 
# you don't need system databases, 
# as mysql 5 and 8 have quite different internal structure
mysqldump -u root -p --databases db1 db2 db3 > data_dump.sql
# !!! AVOID DOING FULL DB BACKUP !!!
# System tables of MySQL 5 and 8 are incompatible
# If you have only all-databases backup ->
# -> open it with text editor and leave only DBs that you need migrate
mysqldump -u root -p --all-databases > all_databases_backup.sql
```

2. Stop and Uninstall MySQL 5:
```bash
brew services stop mysql@5.x # Replace 5.x with your specific MySQL 5 version
brew uninstall mysql@5.x
# and delete old mysql data, to avoid any conflicts in future
rm -rf /opt/homebrew/var/mysql
```

3. Install MySQL 8:
```bash
brew install mysql@8.4
brew services list
```

4. [Optional] Restore Your Databases:
```bash
# need --force to skip all incompatibility errors and skip backupf system tables
mysql -u root --force < all_databases_backup.sql
# if only user databases were backed up --> you don't need to use --force
```

5. [Optional] Secure the MySQL 8 Installation (set root pass and other sec options):
```bash
mysql_secure_installation
```

# DONE !
```
==> Summary
🍺  /opt/homebrew/Cellar/mysql@8.4/8.4.7_2: 322 files, 316.8MB
==> Running `brew cleanup mysql@8.4`...
Disable this behaviour by setting `HOMEBREW_NO_INSTALL_CLEANUP=1`.
Hide these hints with `HOMEBREW_NO_ENV_HINTS=1` (see `man brew`).
==> Caveats
==> mysql@8.4
We've installed your MySQL database without a root password. To secure it run:
    mysql_secure_installation

MySQL is configured to only allow connections from localhost by default

To connect run:
    mysql -u root

mysql@8.4 is keg-only, which means it was not symlinked into /opt/homebrew,
because this is an alternate version of another formula.

If you need to have mysql@8.4 first in your PATH, run:
  echo 'export PATH="/opt/homebrew/opt/mysql@8.4/bin:$PATH"' >> ~/.zshrc

For compilers to find mysql@8.4 you may need to set:
  export LDFLAGS="-L/opt/homebrew/opt/mysql@8.4/lib"
  export CPPFLAGS="-I/opt/homebrew/opt/mysql@8.4/include"

To start mysql@8.4 now and restart at login:
  brew services start mysql@8.4
Or, if you don't want/need a background service you can just run:
  /opt/homebrew/opt/mysql@8.4/bin/mysqld_safe --datadir\=/opt/homebrew/var/mysql
```
