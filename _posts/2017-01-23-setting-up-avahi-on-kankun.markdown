---
layout: post
title:  "Setting up Avahi-Daemon on KanKun"
date:   2017-01-23
comments: true
category: kankun
tags: [linux]
---

Managing devices on a network can be quite cumbersome, especially in an age when IoT is gaining immense popularity. One may want to do something as simple as ping a device, or something more complex like navigate to a hosted web server. With that said remembering statically mapped devices by IP can be daunting, while  hosting a dedicated DNS/DHCP server impractical. One alternative solution called [mDNS](https://en.wikipedia.org/wiki/Multicast_DNS) allows for host name resolution via multicast. The best part is that both Windows (via Apple Bonjour) and Linux (via Avahi) easily support it. This allows us to do things like

```
ping yourdevice.local
ssh username@yourdevice.local
yourdevice.local/index.html
```

For this post I was interested in getting Avahi running on a kankun wifi outlet which had a few caveats to get right. Credit goes to this [OpenWrt ](https://forum.openwrt.org/viewtopic.php?id=56615) post with minor modification for the kankun.

First thing to do is make sure you have a [package manager]({% post_url 2017-01-16-installing-opkg-on-kankun %}) up and running.

Next, install Avahi.

```
opkg update
opkg install avahi-daemon
```

Modify the config file located at  `/etc/avahi/avahi-daemon.conf`
with the following settings:

```
[server]
...
allow-interfaces=wlan
enable-dbus=no

[reflector]
...
enable-reflector=yes
```

Start and enable the service.

```
/etc/init.d/avahi-daemon start
/etc/init.d/avahi-daemon enable
```

At this point it is probably desirable to change the name of the device, especially if there are multiple of the same type on the network. There are [many ways]() to do this on Linux, but I found the following to work best for this device.

```
sysctl -w kernel.hostname=<your_new_device_name>
```

Last but not least, restart Avahi for the changes to take effect.

```
/etc/init.d/avahi-daemon restart
```
