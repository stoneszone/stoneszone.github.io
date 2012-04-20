---
layout: post
title: Arch Desktop Overhaul Part 1 -- New Terminal Emulator
date: 2011-08-06 23:26:11
tags:
- arch_linux
comments: true
---

In the past week or so, mainly due to boredom, I decided to overhaul my Arch setup. This is step one in creating my new desktop.

---

I kept hearing lots of things about urxvt, so I decided to check it out. I had been using sakura, a nice tabbed GTK terminal emulator. Anyway, to install:

{% highlight bash %}
$ sudo pacman -S rxvt-unicode
{% endhighlight %}

Out of the box, urxvt is pretty ugly and has some weird quirks compared to sakura. However, one of the awesome things about it is that you can customize ___everything___. All configuration is basically done through your `~/.Xdefaults` or `~/.Xresources` file.

First, let's change the color scheme:

    URxvt.background: #000000
    URxvt.foreground: #c0c0c0

    ! black
    URxvt.color0: #000000
    URxvt.color8: #2C2C2C
    ! red
    URxvt.color1: #C42B2B
    URxvt.color9: #FF2929
    ! green
    URxvt.color2: #3BD93B
    URxvt.color10: #28FC28
    ! yellow
    URxvt.color3: #E3E336
    URxvt.color11: #FFEA1E
    ! blue
    URxvt.color4: #4747BB
    URxvt.color12: #2121DE
    ! magenta
    URxvt.color5: #CB40CB
    URxvt.color13: #FF00FF
    ! cyan
    URxvt.color6: #6FD2D2
    URxvt.color14: #00FFFF
    ! white
    URxvt.color7: #C0C0C0
    URxvt.color15: #FFFFFF

The colors are in standard HTML hex notation. For each color, there are two values, i.e. color1 and color9 for red. The second is the __bright__ version of that color.

Next, I like to have a transparent terminal. While urxvt has the ability to do true transparency, I prefer not to run a compositing window manager, which is required for that effect. I am perfectly happy with pseudo-transparency, which makes your terminal background semi-transparent, showing the desktop wallpaper beneath it.

    ! enable pseudo-transparency
    URxvt.transparent: true
    ! opacity
    URxvt.shading: 15

urxvt also comes with some ugly scroll bars that I never use anyway, so I disabled them. While we're at it, let's add a nicer looking font.

    URxvt.scrollBar: false
    URxvt.font: xft:Monospace:pixelsize=12

Another issue is how "words" are selected when you double click. For instance, by default, a double click will match a semicolon after a filename in a grep result. The characters that urxvt will split strings on can also be customized.

    URxvt.cutchars: "\\ `\"\'()*;<>[]{|}&,=?@^\:"

One of the best things about urxvt is the ability to write plugins and extensions in simple Perl. urxvt actually ships with several of these extensions, and several other can be found in the AUR and online. One of the cool standard ones allows you to click on URLs in the terminal and open them in the browser. To enable this feature, add the following:

    URxvt.perl-ext-common: default,matcher
    URxvt.urlLauncher: /usr/bin/firefox
    URxvt.matcher.button: 1

The other thing that annoyed me was the difference in mouse and keyboard operation between urxvt and sakura. For instance, I was used to using the mouse's scroll wheel in the terminal and Control+Left/Right to jump a word at a time on the command line and in things like vim. To add the Control+Arrow functionality, use the following:

    URxvt.keysym.Control-Up: \033[1;5A
    URxvt.keysym.Control-Down: \033[1;5B
    URxvt.keysym.Control-Left: \033[1;5D
    URxvt.keysym.Control-Right: \033[1;5C

For the scroll wheel, there is a Perl extension you can install:

{% highlight bash %}
$ yaourt -S urxvt-vtwheel
{% endhighlight %}

And enable it with:

    URxvt.perl-ext-common: default,matcher,vtwheel

Finally, getting the copy and paste functionality to work as desired required some manual configuration. Highlighting something in the terminal and middle-clicking to paste still works as expected. However, Control-Shift-C/V to copy/paste to/from the clipboard did not work. To add this functionality, do the following:

*   Create `/usr/lib/urxvt/perl/clipboard` with the following contents (taken from [here](https://github.com/muennich/urxvt-perls/blob/master/clipboard) and modified to use `xclip`):

{% highlight perl %}
#!/usr/bin/perl

use strict;

sub on_start {
    my ($self) = @_;

    $self->{copy_cmd} = $self->x_resource('copyCommand') || 'xclip -selection clipboard';
    $self->{paste_cmd} = $self->x_resource('pasteCommand') || 'xclip -o -selection clipboard';

    ()
}

sub copy {
    my ($self) = @_;

    if (open(CLIPBOARD, "| $self->{copy_cmd}")) {
        my $sel = $self->selection();
        utf8::encode($sel);
        print CLIPBOARD $sel;
        close(CLIPBOARD);
    } else {
        print STDERR "error running '$self->{copy_cmd}': $!\n";
    }

    ()
}

sub paste {
    my ($self) = @_;

    my $str = `$self->{paste_cmd}`;
    if ($? == 0) {
        $self->tt_paste($self->locale_encode($str));
    } else {
        print STDERR "error running '$self->{paste_cmd}': $!\n";
    }

    ()
}

sub on_user_command {
    my ($self, $cmd) = @_;
    if ($cmd eq "clipboard:paste") {
        $self->paste;
    }
    elsif ($cmd eq "clipboard:copy") {
        $self->copy;
    }
}
{% endhighlight %}

*   Add the following to your `~/.Xdefaults` or `~/.Xresources` as we've been doing:

        URxvt.perl-ext-common: default,matcher,vtwheel,clipboard
        URxvt.iso14755: false
        URxvt.keysym.Shift-Control-V: perl:clipboard:paste
        URxvt.keysym.Shift-Control-C: perl:clipboard:copy

Note that the Perl script above is set up for `xclip`. However, it can be easily adapted to work with any other clipboard manager as desired by changing `copy_cmd` and `paste_cmd`.

That's it! The above configuration gave me a nice urxvt with all the niceties I'm used to, but also leaves lots of room for customization. urxvt has some tabbed modes available, but for now, I'm doing fine with separate terminal windows. Maybe I'll explore those options in the future.

---

The next couple posts will talk more the 'overhaul' I've been performing.

