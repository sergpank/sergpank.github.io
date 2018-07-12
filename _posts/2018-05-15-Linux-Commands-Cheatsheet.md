---
layout: post
title: Linux Commands Cheatsheet
date: 2018-05-15
---  

| Command | Description |
|---------|-------------|
| `find / -name pg_hba.conf`          | Find file 'ph_hba.conf' file starting from the **root** |
| | |      
| `chkconfig --list`                  | List all services |
| `service --status-al`               | List running service |
| `service postgresql96 status`       | Check status of *postgresql96* service |
| `sudo service postgresql96 restart` | Restart *postgresql96* service |
| | |
| `du -hs *`                          | display summarized sized for each file and folder in current directory ("h" - human readable; "s" - summarized |
| | |
| `grep -rni "string" *` | search some string recursively in all files 
where 
r = recursive i.e, search subdirectories within the current directory
n = to print the line numbers to stdout
i = case insensitive search |
| | |
| `yum list ` | show all available packages |
| `yum list installed` | show all installed packages |
| `yum remove <package>` | remove selected package |
| `rpm -qa` | list intalled packages |
| `rpm -ivh <package>` | install selected package |
| `rpm -e <package>` | remove selected package |
| `apt list --installed` | list installed packages |
| | |
| `ln -sfn <full-path> <link-name>` | create or update symbolic link |
| | |
| `gunzip <file.gzip>` | extract **.gzip** or **.gz** archive |
| `tar xvfz <file.tar.gz>` | extract **.tar.gz** archive |
| `tar xvf <file.tar>` | extract **.tar** archive
