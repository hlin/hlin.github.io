---
layout: post
title: Show group members in Django admin site
date: 2016-11-16 11:29:19
---

Django's admin site is very helpful but it's not convenient enough in some cases.
For example, when you want to add several users to a group, you have to edit each
user one by one and you can not see group members in group detail page. Create a
file named *admin.py* with following contents can solve these problems(tested with
Django 1.10).

```python
# -*- coding: utf-8 -*-

from django.contrib import admin
from django.contrib.auth.admin import GroupAdmin
from django.contrib.auth.models import Group
from django.contrib.auth.models import User


class UserInline(admin.TabularInline):
    model = User.groups.through
    extra = 0


class MyGroupAdmin(GroupAdmin):
    inlines = [UserInline]


admin.site.unregister(Group)
admin.site.register(Group, MyGroupAdmin)
```
