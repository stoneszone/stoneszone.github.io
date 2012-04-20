---
layout: post
title: Adding Comments to Nanoblogger
date: 2011-05-14 16:12:25
tags:
- nanoblogger
comments: true
---


First, download [NBCom](http://nhw.pl/blg/articles/nbcom/) to a temporary directory on the web server. I'm using version 1.1. Extract the tarball:

{% highlight bash %}
$ tar xzf nbcom-1.1.tar.gz
$ cd nbcom-1.1
{% endhighlight %}

Follow the instructions in INSTALL.txt to set up the database and your templates appropriately. However, do not add your database password and username to your blog.conf as instructed, as that file is web accessible. Instead, put them in your global nb.conf file, i.e. /etc/nb.conf.

Now, open up the 'install' file, and remove or comment out the line at the top that says:

{% highlight bash %}
BLOG_ADMIN=""
{% endhighlight %}

Close the file. You need to export the password and such as environment variables since we're not putting them in blog.conf.

{% highlight bash %}
$ export BLOG_ADMIN="session"
$ export DB_PASS="my_password"
# Optionally...
$ export DB_HOST="my_db_hostname"
{% endhighlight %}

Now run the install script as instructed:

{% highlight bash %}
$ ./install /path/to/blog
{% endhighlight %}

