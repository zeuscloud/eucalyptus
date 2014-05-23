---
layout: page
title: "Centos 6 OS Installation"
category: doc
date: 2013-06-05 12:00:00
order: 1
---
###Centos 6 Installation 

###Creating a CentOS USB Bootable

Dowload the script from this link and save it as livecd-iso-to-disk.

{% highlight bash %}

chmod +x live-iso-to-disk

./livecd-iso-to-disk --format --reset-mbr /path-to-iso /dev/path-to-usb

{% endhighlight %}

 * Write Centos image to a disc, boot into installation mode
 * Restart the machine after connecting USB
 * Press `F11` to enter boot menu
 * Select USB as boot device
 * Hostname - zeus
 * Root Password - spartan
 * Partition - 100GB root, 40GB swap, rest /var
 * close the installation window
 * user zeus password - spartan
 * start ssh server. service start 

###Error Logs 

* The installation using a bootable usb didn't work in DELL 2950 server even though the bootable USB was working well in laptop.
