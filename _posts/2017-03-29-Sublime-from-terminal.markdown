---
layout: post
title: MacOS - run Sublime from terminal
date: 2017-03-29
---

# MacOS - How to run Sublime Text from command line #

There is an assumption that you have `bin` directory in your home dir and it is registered somewhere in the `$PATH` variable.

In that case you simply have to **create a symlink:**

	ln -s "/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl" ~/bin/subl

Tha's all, now you can start `Sublime Text` from console simply typing:

``subl``

* * *

In case if you don't have `~/bin` directory - **create it**:

``mkdir ~/bin``

And add to your `~/.bash_profile` following line:

``export PATH=$PATH:~/bin``

* * *

ALSO the following plugin is seuper-duper useful: https://packagecontrol.io/packages/Compare%20Side-By-Side
```bash
CMD + Shift + P -> package Control: Install Package -> Compare Side-By-Side
```
I need it as air!

* * *

You can find more details in [SUBLIME TEXT 3 DOCUMENTATION](https://www.sublimetext.com/docs/3/osx_command_line.html "OS X Command Line")
