---
layout: post
title: Generating Sitemaps in Perl
date: 2011-05-14 19:59:43
tags:
- perl
- projects
comments: true
---

I like to keep search engines notified of my website, but I've always had to generate and submit my sitemaps online. Since switching to nanoblogger, I thought it'd be cooler to have something I could run from the command line.

After searching around for awhile, I found some example Perl code [here](http://groups.google.com/group/google-sitemaps/msg/56ba5f933c7bdb70). It was a bit basic for what I needed, so I modified it quite a bit and made it considerably more configurable. What I ended up with was [sitemap-gen](https://github.com/mrstegeman/sitemap-gen).

I'm sure this code could still be improved, but it works great for sites that are strictly HTML, such as nanoblogger. It will generate a proper XML sitemap and automatically submit it to Google, Bing, Yahoo, and Ask. Simple enough.

If you want to try it out, just pull the Perl script from the link above, set the config variables in it, and run it.

