---
layout: page
title: "Eucalyptus Configuration"
category: doc
date: 2013-06-05 12:00:00
order: 2
---

###Configuring the network in Controller
####Opening Ports

{% highlight bash %}

iptables -A INPUT -i eth1 -p tcp --dport 8772 -j ACCEPT
iptables -A INPUT -i eth1 -p tcp --dport 8773 -j ACCEPT
iptables -A INPUT -i eth1 -p tcp --dport 8774 -j ACCEPT
iptables -A INPUT -i eth1 -p tcp --dport 8776 -j ACCEPT
iptables -A INPUT -i eth1 -p tcp --dport 8777 -j ACCEPT
iptables -A INPUT -i eth1 -p tcp --dport 8080 -j ACCEPT
iptables -A INPUT -i eth1 -p udp --dport 8773 -j ACCEPT
iptables -A INPUT -i eth1 -p udp --dport 7500 -j ACCEPT
service iptables save

{% endhighlight %}

####Accepting everything from TCP and UDP

{% highlight bash %}

iptables -A INPUT -p udp --dport 0:1023 -j ACCEPT
iptables -A INPUT -p tcp --dport 0:1023 -j ACCEPT
/sbin/service iptables save

{% endhighlight %}

####Opening ports required for DNS functioning

