---
layout: post
title: Linux Commands Cheatsheet
date: 2018-05-15
---  


### Commands chaining in terminal:
```
A; B    # Run A and then B, regardless of success of A
A && B  # Run B if and only if A succeeded
A || B  # Run B if and only if A failed
A &     # Run A in background.
```

| Command | Description |
|---------|-------------|
| `find / -name pg_hba.conf`          | Find file 'ph_hba.conf' file starting from the **root** |
| `grep -rnw '/path/to/somewhere/' -e 'pattern'` | **-r** is recursive <br> **-n** is line number <br> **-w** stands for match the whole word <br> **-e** is the pattern used during the search |
| `grep -rni "string" *` | search some string recursively in all files where <br> **-r** = recursive i.e, search subdirectories within the current directory <br> **-n** = to print the line numbers to stdout <br> **-i** = case insensitive search |
| | |      
| `chkconfig --list`                  | List all services |
| `service --status-all`               | List running service |
| `service postgresql96 status`       | Check status of *postgresql96* service |
| `sudo service postgresql96 restart` | Restart *postgresql96* service |
| | |
| `du -hs *`                          | display summarized sized for each file and folder in current directory <br> **h** - human readable <br> **s** - summarized|
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
| `zcat x.txt.gz >x.txt` OR `gzcat x.txt.gz > x.txt` OR `gunzip -c x.txt.gz > x.txt` | extract **gzip** without deleting archive|
| `tar cvfz <archive.name.tar.gz> <file.name.1 file.name.2 ...>` | create **tar.gz** archive |
| `tar xvfz <file.tar.gz>` | extract **.tar.gz** archive |
| `tar xvf <file.tar>` | extract **.tar** archive
| `zip -r <zip-name.zip> <directory-to-zip>` | zip a directory |
| `zip <zip-name.zip> <file1> <file2> ...` | zip multiple files into archive |
| `unzip <zip-name.zip>` | unzip a **.zip** archive |
| | |
| `/usr/local/sbin/haproxy -c -V -f /etc/haproxy/haproxy.cfg` | validate HAProxy configuration |
| `service haproxy reload` | update HAProxy configuration |
| | |
| `scp <file_name> <host>:/<path_on_host>` | copy file to remote host |
| | |
| `sslscan <host>` | tests SSL/TLS enabled services to discover supported cipher suites |
| | |
