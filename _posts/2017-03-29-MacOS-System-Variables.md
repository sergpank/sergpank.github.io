---
layout: post
title: MacOS system variables
date: 2017-03-29
---

## Variable Location ##
OSX The default location for custom environmental variables in OSX the **.bash_profile** file.

``~/.bash_profile``

If you can't find this file - you should create it by yourself.

## Listing System Variables ##

To list all the variables that are being used by your system use the **printenv** command.

``printenv``

## Setting a variable ##

To set the variable use the command line.

``CATALINA_HOME=/Users/sergpank/apache-tomcat-8.5.12``

## Reading a variable ##

To read the variable using the command line.

``echo $CATALINA_HOME``
