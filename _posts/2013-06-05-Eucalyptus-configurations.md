---
layout: page
title: "Eucalyptus Configurations"
category: doc
date: 2013-06-05 12:00:00
order: 4
---

In Managed (No-VLAN) mode, Eucalyptus does not use VLANs to isolate the network bridges attached to VMs from each other. Configure each CC to use an Ethernet device that lies within the same broadcast domain as all of its NCs.

To configure for Managed (No VLAN) mode:

###CC Configuration
Important: You must set VNET_PUBLICIPS identically on all CCs in a multi-cluster configuration.
Log in to the CC and open the `/etc/eucalyptus/eucalyptus.conf` file.
Go to the Network Configuration section, uncomment and set the following:
{% highlight bash %}
VNET_MODE="MANAGED-NOVLAN"
VNET_SUBNET="10.0.0.0"
VNET_NETMASK="255.0.0.0"
VNET_DNS="10.0.0.1"
VNET_ADDRSPERNET="64"
VNET_PUBLICIPS="192.168.41.220-192.168.41.230"
VNET_LOCALIP="192.168.41.203"
VNET_DHCPDAEMON="/usr/sbin/dhcpd41"
VNET_DHCPUSER="dhcpd"
{% endhighlight %}

If your NCs are not reachable from end-users directly and the CC has two (or more) Ethernet devices of which one connects to the client/public network and one connects to the NC network, or the single Ethernet device that the CC uses to connect to both clients and NCs is NOT ‘eth0’, then you must also uncomment and set:
{% highlight bash %}
VNET_PRIVINTERFACE="eth1"

VNET_PUBINTERFACE="eth0"
{% endhighlight %}

Save the file.
Repeat on each CC in your system.


###NC Configuration
Log into an NC machine and open the `/etc/eucalyptus/eucalyptus.conf` file.
Go to the Network Configuration section, uncomment and set the following:

{% highlight bash %}
VNET_MODE="MANAGED-NOVLAN"
VNET_BRIDGE="br0"
{% endhighlight %}

Save the file.
Repeat on each NC.


