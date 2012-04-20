---
layout: post
title: Setting Up a Firewall on Arch Linux with UFW
date: 2012-02-18 19:12:00
tags:
- arch_linux
comments: true
---

UFW is by far the easiest front end to iptables that I've ever come across. It comes with sane defaults and several pre-configured application profiles. It makes it super easy for anyone to set up a firewall without learing the ridiculous syntax of iptables. The application profiles used below may or may not be available in other Linux distributions.

To install UFW:

    # pacman -S ufw

Deny all incoming connections by default, outgoing ports remain open.

    # ufw default deny

To allow torrents through Deluge:

    # ufw allow Deluge

To allow incoming SSH connections:

    # ufw allow SSH

For a web server, two options are available. One allows for standard HTTP on port 80, the other for HTTPS on port 443.

    # ufw allow WWW
    # ufw allow "WWW Secure"

For a MySQL server, we have to manually choose a port. I chose to only allow connections on the LAN.

    # ufw allow proto tcp from 192.168.17.0/24 to any port 3306

For a CUPS print server, a profile is available, but I again limited connections to the LAN.

    # ufw allow from 192.168.17.0/24 to any app IPP

DNS servers are also very easy.

    # ufw allow DNS

Allowing incoming connections for a SANE scanner server is a bit more difficult.

*   First, we'll open the necessary TCP port to the LAN:

        # ufw allow from 192.168.17.0/24 to any port 6566 proto tcp

*   Then, we need to add a connection tracker to the UFW config. Open `/etc/default/ufw` and add `nf_conntrack_sane` to the `IPT_MODULES` array.

OpenVPN can be a bit tricky to firewall. Some of the below information was taken from [here](http://blog.philippklaus.de/2010/09/openvpn/).

*   First, open OpenVPN's default UDP port (change this if you're using a different port).

        # ufw allow 1194/udp

*   Now, we have to enable forwarding on the server. Open `/etc/ufw/sysctl.conf` and uncomment:

        net/ipv4/ip_forward=1

*   We also have to make UFW accept forwarded packets. Open `/etc/default/ufw` and change `DEFAULT_FORWARD_POLICY` to `ACCEPT`.

*   Finally, we need to add some custom iptables rules. Open `/etc/ufw/before.rules` and add the following to the very beginning of the file (after the opening comments):

        # nat Table rules
        *nat
        :POSTROUTING ACCEPT [0:0]

        # Forward traffic through tun0 - Change to match your out-interface and IP range.
        -A POSTROUTING -s 10.8.0.0/24 -o eth0 -j MASQUERADE

        # don't delete the 'COMMIT' line or these nat table rules won't be processed
        COMMIT

NFS servers can also be tricky. I'm running a simple, NFSv4 only server, so your setup may vary a bit.

*   First, open `/etc/conf.d/nfs-common.conf` and set a static port for statd, gssd, and sm-notify if necessary. In my case, this was not necessary, but you can check if those servers are running with `rpcinfo -p`.

*   Next, open `/etc/conf.d/nfs-server.conf`. Set a static port for mountd, i.e. add `--port 20048` to `MOUNTD_OPTS`.

*   We then need to set static ports for lockd, which is a kernel module. Open up `/etc/modprobe.d/lockd.conf` and add the following:

        options lockd nlm_udpport=4001 nlm_tcpport=4001

*   We can now open the ports we need:

        # ufw allow NFS
        # ufw allow 20048
        # ufw allow 4001

Finally, enable and start ufw.

    # ufw enable

Make sure you also add `ufw` to your `DAEMONS` array in `/etc/rc.conf`.

To check your current rules, you can use:

    # ufw status verbose
