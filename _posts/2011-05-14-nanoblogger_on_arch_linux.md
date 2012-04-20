---
layout: post
title: Nanoblogger on Arch Linux
date: 2011-05-14 12:32:10
tags:
- arch_linux
- nanoblogger
comments: true
---

First, install nanoblogger:

{% highlight bash %}
$ sudo pacman -S nanoblogger
{% endhighlight %}

Edit /etc/nb.conf and set the BLOG_DIR variable, i.e.

{% highlight bash %}
BLOG_DIR="/srv/http/blog"
{% endhighlight %}

Now, you'll need to actually create the blog:

{% highlight bash %}
$ nb add weblog
{% endhighlight %}

You should probably edit the configuration when asked and set whatever you need to.

Start adding entries:

{% highlight bash %}
$ nb add entry
{% endhighlight %}

