---
layout: post
title: Better, Integrated Comments for Nanoblogger
date: 2011-05-18 18:48:53
tags:
- nanoblogger
- projects
comments: true
---

After setting up nanoblogger, I experimented with several different comment add-ons/plugins. However, they all had their own issues.

*   NBCom, previously discussed, does not integrate very well at all with nanoblogger. It has a completely different look, and it uses PHP and MySQL, which goes against the idea behind nanoblogger.

*   Blogkomm uses some crufty old PHP4 scripts and has very poor documentation, as well as an admin tool that I could not get to work.

*   CGIComment, JS-Kit, and Haloscan, all of which are mentioned in the nanoblogger manual, seem to have vanished.

So, I decided to write my own comment system. In the spirit of nanoblogger, it's a Bash-based system that stores plaintext files for every comment, organized similarly to blog posts. The comments are stored as such:

    BLOG_DIR/comments/NB_EntryID/<comment number>.txt


It really only required me to change about 3 lines in the main `nb` script, and I had to slightly modify a few of the templates and CSS styles.

The main workhorse is a Bash CGI script, right at 100 lines of code. I'm sure it could probably be improved upon, but it's working for me.

Rather than write a whole instruction manual on setting this up, I decided to just push it all up to [github](https://github.com/mrstegeman/nanoblogger-mod). If you want to use this, just pull the code from there and install it the same way that you would normally install nanoblogger. I also included the theme for my blog in that repo.

Two caveats:

1.  Your webserver must be set up to allow CGI scripts to run from `BLOG_DIR/cgi-bin/`. An example for your apache.conf/http.conf/vhosts.conf/etc. is as follows:

        ScriptAlias /cgi-bin/ /srv/http/www.thestegemans.com/cgi-bin/
        <Directory "/srv/http/www.thestegemans.com/cgi-bin">
            AllowOverride None
            Options +ExecCGI -MultiViews +SymLinksIfOwnerMatch
            Order allow,deny
            Allow from all
        </Directory>

2.  Javascript must be enabled in the browser for comments to appear.

I may try to get the main nanoblogger project to incorporate these changes soon.

Happy commenting!

