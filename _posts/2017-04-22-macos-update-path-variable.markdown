---
layout: post
title: MacOS - update PATH variable
date: 2017-04-22
categories: mac howto
---

## Update $PATH for single user ##

To update `$PATH` variable for single user you just have to add one line to `~/.bash_profile` file (for example lets add bin dir of PostgreSQL):

{% highlight shell %}
export PATH=$PATH:/Library/PostgreSQL/9.6/bin
{% endhighlight %}

Restart terminal and check if it has worked:

{% highlight shell_session %}
$ echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Users/sergpank/bin:/Library/PostgreSQL/9.6/bin
$ which psql
/Library/PostgreSQL/9.6/bin/psql
$ psql --version
psql (PostgreSQL) 9.6.2
{% endhighlight %}

## Update $PATH for all users ##

To update `$PATH` variable for all system users, you have to edit `/etc/paths` file.

Lets add PostgreSQL to PATH again:

{% highlight shell_session %}
$ sudo vi /etc/paths
.
.
and simply add one more line at the end of the file:
/Library/PostgreSQL/9.6/bin
.
.
[ESC]:wq
.
.
$ cat /etc/paths
/usr/local/bin
/usr/bin
/bin
/usr/sbin
/sbin
/Library/PostgreSQL/9.6/bin
{% endhighlight %}

Now restart terminal and check that `$PATH` variable is modified.

* * *

Inspiration [nixCraft: Mac OS X: Set / Change $PATH Variable][nixCraft article]

[nixCraft article]: https://www.cyberciti.biz/faq/appleosx-bash-unix-change-set-path-environment-variable/