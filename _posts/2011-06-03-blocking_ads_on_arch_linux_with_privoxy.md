---
layout: post
title: Blocking Ads on Arch Linux with Privoxy
date: 2011-06-03 19:41:12
tags:
- arch_linux
comments: true
---

Installing an ad blocking extension, such as AdBlock Plus, for every browser gets to be annoying, and it's unavailabe for some browsers. They can also slow down your browser.

Instead of doing this, why not install a system-wide ad blocker? This can be achieved by installing an ad blocking proxy, such as Privoxy, and just setting a proxy address for each of your browsers.

{% highlight bash %}
# install privoxy
$ sudo pacman -S privoxy

# (optional) install an AdBlock Plus easylist importer from AUR
# replace yaourt with whatever you use for AUR installation
$ yaourt -S privoxy-blocklist
$ sudo privoxy-blocklist

# start privoxy
$ sudo /etc/rc.d/privoxy start
{% endhighlight %}

Next, you'll need to add privoxy to your DAEMONS array in your `/etc/rc.conf`, i.e.

{% highlight bash %}
DAEMONS=(... @privoxy)
{% endhighlight %}

## Browser configuration:

### Chrome/Firefox

The easiest way to set the proxy for Chrome and Firefox is by adding an environment variable
    *   In Chrome, the proxy can't even be set through the browser.

To do this, create `/etc/profile.d/privoxy.sh` with the following:

{% highlight bash %}
export http_proxy="http://127.0.0.1:8118"
export https_proxy="http://127.0.0.1:8118"
{% endhighlight %}

Change the permissions on the file:

{% highlight bash %}
$ sudo chmod a+x /etc/profile.d/privoxy.sh
{% endhighlight %}

You'll need to log out and back in for the changes to take effect.

For Firefox, you'll need to do the following:
*   Preferences-&gt;Advanced-&gt;Network-&gt;Settings
*   Select "Use system proxy settings"

If this doesn't work for Firefox (as has happened to me...), you can manually configure Firefox's proxy settings:
*   Preferences-&gt;Advanced-&gt;Network-&gt;Settings
*   Select "Manual proxy configuration"
*   Set HTTP and SSL proxy host to 127.0.0.1 and port to 8118.

### Opera

*   Menu-&gt;Settings-&gt;Preferences-&gt;Network
*   Click on "Proxy Servers..."
*   Set host to 127.0.0.1 and Port to 8118 for HTTP and HTTPS.

### uzbl

In your `~/.config/uzbl/config`, add the following:

    set proxy_url = http://127.0.0.1:8118

