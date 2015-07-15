---
layout: post
title: Setting up Ghost on a Raspberry Pi
date: 2015-01-16 02:13:22.000000000 +01:00
tags: RaspberryPi Ghost
---
It is fascinating that this small chip-computer could do so much.
Following is how I installed this Ghost Blog on this RPi.


####1st step

{% highlight bash %}
$ wget http://nodejs.org/dist/node-latest.tar.gz
$ tar -xzf node-latest.tar.gz
$ cd [node folder]
{% endhighlight %}

download node and unarchive it.

####2nd step

{% highlight bash %}
$ ./configure  
$ make  
$ make install
{% endhighlight %}

compile node and install it. please note the `make` could take hours, consider using tmux to put it to a separate session.

####3rd step

{% highlight bash %}
$ wget https://ghost.org/zip/ghost-latest.zip
$ unzip -d ghost [Name-of-Ghost-zip].zip
{% endhighlight %}

download and install Ghost.

####4th Step

{% highlight bash %}
$ cd ghost/
$ sudo npm install
$ cp config.example.js config.js
{% endhighlight %}

note that `sudo npm install` could take some time as well.

Here change all instance of `host: '127.0.0.1'` and `port: '2368'` to `host: '[ip of RPi]'` and `port: '80'`, then we can reach ghost from internet.

Finally we can start Ghost with `npm start`, please note that we are starting Ghost with default user `pi`, so that binding port under `1024` is impossible, in this case you will need to run this command with `sudo`.
