---
layout: post
title: Comment Notifications by E-mail for nanoblogger-mod
date: 2011-05-24 20:55:30
tags:
- nanoblogger
- projects
comments: true
---

After getting integrated comments working for nanoblogger-mod, I thought it'd be nice to have some notifications e-mailed to me when I get new blog comments. I've decided not to add this to the nanoblogger-mod source, as it leads to a much more confusing installation. Instead, I'm just putting the instructions here if anyone wants a similar setup.

Again, this is written all in Bash, so it can be customized however you want.

First, if you don't have a way to send mail from your system (this was the case for me), you'll need to set that up. This can be done by installing something like `heirloom-mailx` on Arch or `mailx` on Ubuntu. After installing that, edit your `~/.mailrc` file and set up an SMTP forward. The following will work for Gmail.

    set smtp-use-starttls
    set smtp=smtp://smtp.gmail.com:587
    set smtp-auth=login
    set smtp-auth-user=<myusername>@gmail.com
    set smtp-auth-password=<mypass>
    set from="<myusername>@gmail.com"


Close that file. For security's sake, change the permissions so other users can't read it, i.e. `chmod 600 ~/.mailrc`. To make sure it works, you can do something like this:

{% highlight bash %}
$ echo 'Hello, world' | mail -s 'Testing Mail' <myusername>@gmail.com
{% endhighlight %}

If you got an e-mail, keep going. Otherwise, you'll need to look up how to get e-mail working properly. That's beyond the scope of this post.

The other step is to create a script that will gather new comments and have it executed periodically by cron. The following is working well for me:

{% highlight bash %}
#!/bin/bash

BLOG_DIR="/path/to/blog/dir"
TMP_DIR="/var/tmp"
TMP_FILE="/tmp/nb_com.tmp"
TO_ADDR="<myusername>@gmail.com"
SUBJECT="Blog Comments"

find "$BLOG_DIR/comments/" -name '*.txt' | sort > "$TMP_DIR/nb_com_cur.dat"

if [ -f "$TMP_FILE" ]; then
rm "$TMP_FILE"
fi

num=0

for file in $(diff "$TMP_DIR/nb_com.dat" "$TMP_DIR/nb_com_cur.dat" | egrep '^>' | cut -d ' ' -f 2- | sed -e 's/\/[0-9]*\.txt$/\.txt/' | uniq)
do
num=$(($num+1))
    entry=$(echo "$file" | sed -e 's/\/comments\//\/data\//')

    if [ -r "$entry" ]; then
title=$(egrep '^TITLE:' "$entry" | sed -e 's/^TITLE:\s*//')
        echo "$title" >> "$TMP_FILE"
    fi
done

if [ "$num" -gt 0 ]; then
entries=$(< $TMP_FILE)
    echo -e "You have new blog comments on the following posts:\n" > "$TMP_FILE"
    echo "$entries" >> "$TMP_FILE"
    cat "$TMP_FILE" | mail -s "$SUBJECT" "$TO_ADDR"
fi

mv "$TMP_DIR/nb_com_cur.dat" "$TMP_DIR/nb_com.dat"
{% endhighlight %}

The script above will send out an e-mail containing something like this:

    You have new blog comments on the following posts:

    Adding comments to nanoblogger
    Better, integrated comments for nanoblogger

Save the script somewhere and make it executable, i.e. `chmod a+x my_script`. You then need add a cron entry to execute the script periodically. Two examples are below.

*   To add an entry manually that runs every hour, run `crontab -e` and add something like the following:

        0 * * * * /path/to/my_script

*   If your system is set up to run scripts in a folder on a given time interval, you could just simply put the script there. For instance, on Arch Linux, the root user's crontab looks like this:

        01 * * * *  /usr/sbin/run-cron /etc/cron.hourly
        02 00 * * * /usr/sbin/run-cron /etc/cron.daily
        22 00 * * 0 /usr/sbin/run-cron /etc/cron.weekly
        42 00 1 * * /usr/sbin/run-cron /etc/cron.monthly

    *   So, to make the script run hourly, make sure it's executable and put it in `/etc/cron.hourly/`.

