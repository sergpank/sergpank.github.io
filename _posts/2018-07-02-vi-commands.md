---
layout: post
title: VI commands
date: 2018-07-02
---  

| Command | Description |
|---------|-------------|
| '/' or '?' + word | search for some word /you ?test - will search for 'you' or 'test' words. 'n' or '*' - next occurence. 'shift + n' or '#' - previous occurence. |
| | |
| :set number | show line numbers |
| :set nonumber | hide line numbers |
| | |
| :q | quit |
| :q! | qit and discard all changes |
| :w | write changes |
| :wq | write changes and quit |
| :x | write changes and quit |
| ZZ | write changes and quit
| | |
| i |	insert at the current position |
| I |	insert at the beginning of line |
| a |	append just after the current cursor position |
| A |	append at the end of line |
| o |	Open a new line below the current line |
| O |	Open a new line above the current line |
| | |
| yy | copy line |
| dd | cut line |
| p | insert after current line |
| P | insert before current line |
| | |
| u | undo last action |
| ctrl + r | redo last action |
| U | revert last line to original state |
| | |
| :E or :Ex or :Explore | Exit back to directory list when opened directory in VIM as vim . |

