---
layout: post
title: custom PS1 for perlbrew
date: 2013-11-25 13:38:13 +0800
---

Add following code to `~/.bashrc`, then run `source ~/.bashrc`

```shell
__perlbrew_ps1()
{
    if hash perlbrew; then
        local v="$( perlbrew list | perl -lane'print $F[1] if /^\*/' )"
        if [[ -n "$v" ]]; then
            printf "${1:- (%s)}" $v
        fi
    fi
}

PS1='[\u@\h$(__perlbrew_ps1) \W$]\$ '
```

Your PS1 will looks like:

```shell
[hlin@desktop-hlin (perl-5.10.1) ~]$
```

