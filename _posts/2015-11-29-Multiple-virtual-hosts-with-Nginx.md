---
layout: post
title: Multiple virtual hosts with Nginx
date: 2015-11-29 19:15:00.000000000 +01:00
tags: nginx
---

When we develop web applications, we have to test on our local boxes firstly.
In this case it always comes in handy if we could use a web server to serve the
application just like we plan to use on production server. When we have many
apps, a step further, when we want them talking to each other, we will have
to use virtual hosts. And this is quite simple in nginx.

Here's one simple config file for 2 virtual hosts in nginx.

{% highlight nginx %}
http {
  include           mime.types;
  default_type      application/octet-stream;

  sendfile          on
  keepalive_timeout 65;
  gzip              on;
  gzip_disable      msie6;
  gzip_comp_level   6;
  gzip_min_length   1100;
  gzip_buffers      16 8k;
  gzip_proxied      any;
  gzip_types        text/plain
                    application/xml
                    text/css
                    text/js
                    text/xml
                    application/x-javascript
                    text/javascript
                    application/json
                    application/xml+rss;
  gzip_vary         on;

  server {
    listen 80;
    server_name     app1.dev *.app1.dev;
    root            /path_to_app1_root/public;
    index           index.php;
  }

  server {
    listen 80;
    server_name     app2.dev *.app2.dev;
    root            /path_to_app2_root/public;
    index           index.py
  }
}
{% endhighlight %}

Now let's break it down.

Firstly is the top-level block, we are defining a http server, which has many default
settings, the served types, gzip setting and such. All these you can find in official
manual.

The most important part is the code below.
{% highlight nginx %}
server {
  listen 80;
  server_name     app1.dev *.app1.dev;
  root            /path_to_app1_root/public;
  index           index.php;
}

server {
  listen 80;
  server_name     app2.dev *.app2.dev;
  root            /path_to_app2_root/public;
  index           index.py
}
{% endhighlight %}

Here we defined two virtual hosts.

- Both of them listen to port 80,
- check the requested server with `server_name`, if matched, process it.
- then it should deliver result from `root` with content of `index`

![two-hosts](https://cloud.githubusercontent.com/assets/775611/11459658/e879d474-96db-11e5-984d-a6198415c7b4.png)

In this above example we only serve 2 static pages, but you more or less get the
idea how the structure of this config should look like.


----


What if we actually want to serve few dynamic sites? Of course that's also possbile!
Inside each server block you need to tell nginx how should it talk to your apps,
for example with following php app running with php-fpm:

{% highlight php-fpm %}
listen = /path_to_php-fpm.sock
{% endhighlight %}

Here we let nginx talk to php-fpm with unix-socket, for example in app1:

{% highlight nginx %}
server {
  listen 80;
  server_name     app1.dev *.app1.dev;
  root            /path_to_app1_root/public;
  index           index.php;

  # If file does not exist invoke index.php
  location / {
    try_files $uri $uri/ /index.php?$args;
  }

  # Forward PHP files to FPM
  location ~ \.php$ {
    try_files $uri =404;
    fastcgi_read_timeout 14400;
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    fastcgi_pass unix:/path_to_php-fpm.sock;
    fastcgi_index index.php;
    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param OPENDI_ENV local;
    fastcgi_param SERVER_NAME "App1 on my local box";
  }
}
{% endhighlight %}

Above are just simple settings for 2 apps, you can extend them to your usage and
make several apps running at same time on your machine :) Please leave a comment
if you have any question!
