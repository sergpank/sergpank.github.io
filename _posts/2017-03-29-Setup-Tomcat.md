---
layout: post
title: MacOS - Setup Tomcat
date: 2017-03-29
---

This is extremely easy:

1. Download and install [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
2. Download [Tomcat binaries](http://tomcat.apache.org/download-80.cgi) from the official site.
3. Unpack archive into your home folder i.e: `~/apache-tomcat-8.5.12`
4. Set `CATALINA_HOME` variable:

	Add to **~/.bash_profile** `export CATALINA_HOME=~/apache-tomcat-8.5.12` 

5. Set `JAVA_HOME` variable:

	Add to **~/.bash_profile** `export JAVA_HOME=$(/usr/libexec/java_home)`

Rest of actions is optional.

* * *

**To start Tomcat run** `./startup.sh`

**To stop Tomcat run** `./shutdown.sh`

If scripts do not start add permission for execute: `chmod +x <filename>`

After startup, you can access rinning web app at `http://localhost:8080/`