---
layout: post
title: Migrate Homebrewed MySQL from OS X to macOS
date: 2016-09-24 21:23:56.000000000 +01:00
tags: MySQL OSX macOS
---

![mysql](https://cloud.githubusercontent.com/assets/775611/18817718/bdb52116-8367-11e6-9312-6ebccfc2ec8a.png)
![homebrew](https://cloud.githubusercontent.com/assets/775611/18817715/aea511cc-8367-11e6-8d3f-cd57d78b708e.png)

So Apple has released newer version of its OS, reusing the old name macOS. As
always I upgrade my working system to macOS, then [homebrewed](http://brew.sh/)
MySQL won't work anymore, to be precise, it cannot do `write` anymore.

In this post I will write down what I've done to make it work again, even with all
data kept. Hope it will help someone digging around for hours but still no clue.

Basically what I do is to reinstall [MySQL](https://www.mysql.com/) with homebrew.
As usual I did a upgrade with:

{% highlight bash %}
$ brew upgrade mysql
{% endhighlight %}

but no, no difference, still cannot write.

Then I removed MySQL with:

{% highlight bash %}
$ brew uninstall mysql
$ brew prune mysql
$ brew cleanup
{% endhighlight %}

After this all MySQL binaries are removed from my system, but still there were
database default files and databases left in `/usr/local/var/mysql`. And here the
databases and innodb files should be kept.

I copied all these to another place and delete every other thing:

{% highlight bash %}
$ cp /usr/local/var/mysql/ib* /path/to/my_back_up_folder/.
$ cp -R /usr/local/var/mysql/{database1, database2} /path/to/my_back_up_folder/.
$ rm -rf /usr/local/var/mysql
{% endhighlight %}

Then I did a fresh MySQL install with homebrew and copied back all the backuped
files, start MySQL:

{% highlight bash %}
$ homebrew install MySQL
$ cp -R /path/to/my_back_up_folder/{database1, database2} /usr/local/var/mysql/.
$ brew services start mysql
{% endhighlight %}

Finally MySQL is working, all my old databases are all kept!
