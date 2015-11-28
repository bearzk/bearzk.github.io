---
layout: post
title: Setting up xdebug for PhpStorm on OSX
date: 2015-11-28 20:40:00.000000000 +01:00
tags: PHP OSX xdebug
---

![xdebug](https://cloud.githubusercontent.com/assets/775611/11454143/7d2e016c-9623-11e5-817c-2b1a21a9d7f8.png)

Maybe it was only me, but it really took me quite long to figure out how to enable
xdebug for Phpstorm and debug my php code with it, now I have to record this whole
process.

### 1. Install PHP and xdebug

The PHP comes with the OSX isn't optimal for what we want to do here, since we plan
to use [Homebrew](http://brew.sh/) as our package management software as much as
possible. You can install PHP with following command:

{% highlight bash %}
$ brew install php56 php56-xdebug
{% endhighlight %}

### 2. Enable xdebug

Now the xdebug is installed, we want to enable it. The `ini` file for brewed PHP
should be at `/usr/local/etc/php/5.6/conf.d/ext-xdebug.ini`. It should contain:

{% highlight ini %}
[xdebug]
zend_extension="/usr/local/opt/php56-xdebug/xdebug.so"

xdebug.remote_enable = 1
xdebug.remote_port = 9000

xdebug.profiler_enable = 1
xdebug.profiler_output_dir="/tmp"
xdebug.profiler_enable_trigger = 1
{% endhighlight %}

here the very important attributes are `xdebug.remote_enable` and `xdebug.remote_port`.

### 3. Configure PhpStorm

In Phpstorm the configuration for xdebug is at `Preference > Languages & Framework > PHP > Debug > Xdebug`
remember to change the debug port to what we have in previous step and mark `Can accept external connections`.

![PhpStorm Preference](https://cloud.githubusercontent.com/assets/775611/11454124/e077fc9c-9622-11e5-9b40-bbf7f5c799b0.png)

### 4. Debug with Xdebug in PhpStorm

Now open your project and click on the `listen to the debug button connections`
in PhpStorm.

![Listing to the debug connections](https://cloud.githubusercontent.com/assets/775611/11454151/d682414c-9623-11e5-953c-27c1d2fa3c88.png)

Set a break point by click on the gutter of your code. Then start your php server
and open the page you want to debug, append this query `?XDEBUG_SESSION_START=true`
at the end of the url, refresh page ...

![url](https://cloud.githubusercontent.com/assets/775611/11454203/ef3043c8-9624-11e5-8f51-d876f7799ca1.png)

Voila, now you can debug your code in PhpStorm with xdebug!

![xdebug in phpstorm](https://cloud.githubusercontent.com/assets/775611/11454229/64dcfe72-9625-11e5-97ee-aef34944ce25.png)
