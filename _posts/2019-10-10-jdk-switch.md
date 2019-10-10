---
layout: post
title: Switch java versions in command line
date: 2019-10-10
category: mac
---  

Let's assume that you have the installed java7 and java8 on your mac.

Now you need to setup the following aliases (in `~/.bash_profile`):
```bash
alias j7='export JAVA_HOME=`/usr/libexec/java_home -v 1.7`'
alias j8='export JAVA_HOME=`/usr/libexec/java_home -v 1.8`'
```

`java_home` util will pick up the latest (corresponding) java version from `/Library/Java/JavaVirtualMachines` dir and will update all necessary symlinks and env variables.

That's it!
