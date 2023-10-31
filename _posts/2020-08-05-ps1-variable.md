---
layout: post
title: PS1 Variable
date: 2020-08-05
category: commandline
---  

## I prefer to set it the following way for Bash:
```bash
export DEFAULT_PS1='[\t] > [\u] > [\w] >\n'

echo $PS1
[\t] > [\u] > [\w] >\n

# it will look like: 
# [15:13:31] > [sergpank] > [~/test/Ruby] >
```
## And the following pattern for Zsh:
```
function parse_git_branch() {
    git branch 2> /dev/null | sed -n -e 's/^\* \(.*\)/[\1]/p'
}

COLOR_DEF=$'%f'
COLOR_TIME=$'%F{243}'
COLOR_DIR=$'%F{197}'
COLOR_GIT=$'%F{39}'
setopt PROMPT_SUBST
export PROMPT='${COLOR_TIME}%* ${COLOR_DIR}%~ ${COLOR_GIT}$(parse_git_branch)${COLOR_DEF} $ '

# it will look like:
# 15:19:14 ~/powerlevel10k [master] $
```
