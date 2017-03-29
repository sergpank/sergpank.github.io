---
layout: post
title: MacOS system variables
date: 2017-03-29
---

# MacOS System Variables Setup #

## Variable Location ##
OSX The default location for custom environmental variables in OSX the *.bash_profile* file.

``~/.bash_profile``

To list all the variables that are being used by your system use the `printenv` command.

``printenv``

## Setting a variable ##

To set the variable use the command line.

``export NODE_ENV=development``

## Reading a variable ##

To read the variable using the command line.

``echo NODE_ENV``
