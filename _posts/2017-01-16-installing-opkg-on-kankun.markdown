---
layout: post
title:  "Installing opkg on KanKun"
date:   2017-01-16
category: kankun
tags: [linux]
comments: true
---

After playing around a bit, I realized I needed a package manager to do some basic stuff. I was specifically interested in getting mdns (avahi) up and running so that network device management could be performed more intuitively by name without fumbling with a dns server. I initially found a [KanKun Manager](https://github.com/homedash/kankun-manager) project on GitHub which could be used to install the package manager, but found myself installing dependencies and hacking scripts to get it to work properly (credit is given to the author for providing the usable package below). Here are the steps involved with installing opkg (using a mac):

1. Download the re-hosted package [here](https://github.com/dougcooper/kankun/blob/master/opkg-rc3.tar.gz)
2. Copy the file over to the KanKun using the following command, while entering the necessary passwords

   ```bash
   sudo scp <location_of_opkg> root@<kankun_ip_address>:/tmp/
   ```

    Note 1: `scp` was used since `rsync` is unavailable on the target system.

    Note 2: At this point I should mention that `df` can be used to query the available system storage. By locating temporary files in `/tmp` we wont eat at the main root filesystem where executables will be installed. It is also worth mentioning that upon reboot, `/tmp` is cleared so make sure you have a backup of whatever you put there.

3. `ssh` into the device and un-tar the file

   ```bash
   cd /tmp
   ```

   ```bash
   tar -xzf opkg-rc3.tar.gz && cd opkg-rc3
   ```

4. Move the contents to the appropriate location

   ```bash
   mv bin/opkg-rc3 /bin
   ```

   ```bash
   mv etc/opkg.conf /etc
   ```

   ```bash
   opkg update
   ```

5. Happy installing! More info about opkg can be found on the [OpenWrt](https://wiki.openwrt.org/doc/techref/opkg) site.
