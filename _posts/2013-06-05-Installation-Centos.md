---
layout: page
title: "Centos 6 OS Installation"
category: doc
date: 2013-06-05 12:00:00
order: 1
---
###Centos 6 Installation 

###Creating a CentOS USB Bootable

Go to BIOS setting where you set the USB emulating mode to Hard disk

Write the iso image to USB.

{% highlight bash %}

dd if=/path-to-iso of=/path-to-usb

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

