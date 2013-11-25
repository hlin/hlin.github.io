---
layout: post
title: custom PS1 for perlbrew
date: 2013-11-25 13:38:13
---

Add following code to `~/.bashrc`, then run `source ~/.bashrc`

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

your PS1 will looks like:

    [hlin@desktop-hlin (perl-5.10.1) ~]$

