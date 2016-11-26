---
layout: post
title: Github pages custom domain with https support
date: 2016-11-26 22:52:55 +0800
---

TL;DR
-----

- Must have a VPS and a domain name
- Make sure your domain name pointing to your VPS' IP
- Apply ssl cert from Let’s Encrypt
- Config nginx

Apply ssl cert from Let’s Encrypt
---------------------------------

We will use [certbot][] to apply ssl cert from [Let's Encrypt][].
For CentOS/RHEL7, `certbot` is available in EPEL.

```shell
$ sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
$ sudo yum install certbot
```

Apply cert for my domain hypo.name

```shell
$ sudo certbot certonly --standalone -d hypo.name
```

The cert files will be saved in directory `/etc/letsencrypt/live/hypo.name/` (read [letsencrypt doc][] for more details).

Config nginx
------------

Install nginx

```shell
$ sudo yum install nginx
$ sudo systemctl enable nginx
```

Add configuration file `/etc/nginx/conf.d/hypo.name.conf` with following contents

```
server {
    listen 80;
    server_name hypo.name;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name hypo.name;
    ssl_certificate /etc/letsencrypt/live/hypo.name/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/hypo.name/privkey.pem;
    location / {
        proxy_pass https://hlin.github.io;
        proxy_redirect off;
    }
}
```

Restart nginx

```shell
$ sudo systemctl restart nginx
```

Renew cert automatically
------------------------

Let's Encrypt certs will expire in 90 days, so will use crontab to renew it automatically.

```shell
$ sudo crontab -e
0 0 1 * * certbot renew --pre-hook "systemctl stop nginx" --post-hook "systemctl start nginx" --quiet
```

[certbot]: https://certbot.eff.org/
[Let's encrypt]: https://letsencrypt.org/
[letsencrypt doc]: http://letsencrypt.readthedocs.io/en/latest/using.html#where-are-my-certificates
