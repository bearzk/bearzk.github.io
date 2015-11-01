---
layout: post
title: Create laravel project on local machine
date: 2015-01-16 19:21:00.000000000 +01:00
tags: PHP laravel OSX
---

Today my wife asked me to set up `laravel` on her box, so she could finally start
to learn some serious PHP.

Then I asked a question: "There must be tutorials or docs for such task. Have
you check them all out?"

"Yes, I have. But the official site told me to install some vagrant thingy first,
I don't understand what that is and why."

Then I briefly browsed the `laravel` doc, section installation. Truly, vagrant
is a good option for experienced developer because of many reasons, but for
starter, can you just help them to start an example `laravel` app?

Within 10 mins, including downloading and installing time, I already got an
`laravel` instance running on her machine, without using any virtual machine or
similar. following is how it was set up on OSX:

### Install php and composer
If you don't have `php`, `composer` and `mysql`, then just install them with:

{% highlight bash %}
$ brew install php56 mysql
$ curl -sS https://getcomposer.org/installer | php
{% endhighlight %}

### Install laravel installer

{% highlight bash %}
$ composer global require "laravel/installer=~1.1"
{% endhighlight %}

### Create a laravel app

{% highlight bash %}
$ laravel new myapp
{% endhighlight %}

### Just start it

{% highlight bash %}
$ cd myapp
$ php artisan serve
{% endhighlight %}

yes, as this simple. I understand docs want to learner to have the exactly same
perfect env like an app would be deployed on. But for a total beginner, to actually
start something is more important than knowing how it should be properly developed.
