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
Log in to the CC and open the /etc/eucalyptus/eucalyptus.conf file.
Go to the Network Configuration section, uncomment and set the following:
VNET_MODE="MANAGED-NOVLAN"
VNET_SUBNET="[Subnet for VMs private IPs. Example: 192.168.0.0]"
VNET_NETMASK="[Netmask for the vnet_subnet. Example: 255.255.0.0]"
VNET_DNS="[DNS server IP]"
VNET_ADDRSPERNET="[Number of simultaneous instances per security group]"
VNET_PUBLICIPS="[Free public IP 1] [Free public IP 2] ..."
VNET_LOCALIP="[IP address that other CCs can use to reach this CC]"
VNET_DHCPDAEMON="[Path to DHCP daemon binary. Example: /usr/sbin/dhcpd3]"
VNET_DHCPUSER='[DHCP user. Example: dhcpd]"
If your NCs are not reachable from end-users directly and the CC has two (or more) Ethernet devices of which one connects to the client/public network and one connects to the NC network, or the single Ethernet device that the CC uses to connect to both clients and NCs is NOT ‘eth0’, then you must also uncomment and set:
VNET_PRIVINTERFACE="[Ethernet device on same network as NCs. Example: eth1]"

VNET_PUBINTERFACE="[Ethernet device on ‘public’ network. Example: eth0]"
Save the file.
Repeat on each CC in your system.


###NC Configuration
Log into an NC machine and open the /etc/eucalyptus/eucalyptus.conf file.
Go to the Network Configuration section, uncomment and set the following:
VNET_MODE="MANAGED-NOVLAN"
VNET_BRIDGE="[bridge name. Example: br0]"
Save the file.
Repeat on each NC.


