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

###Network Configurations in Main node ( CC / CLC / Walrus ) 

#####Eth0 - Public network Interface

<table border="1">
    <tr>
        <td>IP</td>
        <td>192.168.41.203</td>
    </tr>
    <tr>
        <td>gw</td>
        <td>192.168.41.1</td>
    </tr>
    <tr>
        <td>dns</td>
        <td>192.168.254.2</td>
    </tr>
    <tr>
        <td>Netmask</td>
        <td>24</td>
    </tr>
</table>

#####Eth1 - Private network Interface
<table border="1">
    <tr>
        <td>IP</td>
        <td>10.0.0.1</td>
    </tr>
    <tr>
        <td>gw</td>
        <td>10.0.0.1</td>
    </tr>
    <tr>
        <td>dns</td>
        <td>192.168.254.2</td>
    </tr>
    <tr>
        <td>Netmask</td>
        <td>24</td>
    </tr>
</table>


###Check Network 

#####Install git and java

{% highlight bash %}
yum install java-devel git git-core
{% endhighlight %}

#####Network Check Script 
Test multicast connectivity between each CLC and Walrus, SC, and VMware broker host.
* Clone the Eucalyptus deveutils repository

`git clone https://github.com/eucalyptus/deveutils`

* Run the network-tomography tool on the Cloud Controller, Cluster Controller, Storage Controller, and any
machines running Walrus or VMware Broker, passing a list of IP addresses for each of these machines.

{% highlight bash %}
cd deveutils/network-tomography
./network-tomography 10.0.0.1 10.0.0.2
{% endhighlight %}

###Configure Dependencies
Before you install Eucalyptus, make sure you have the following dependencies installed and configured.
####Configure Bridges

For Managed (No VLAN), Static, and System modes, you must configure a Linux ethernet bridge on all NC machines.This bridge connects your local ethernet adapter to the cluster network. Under normal operation, NCs will attach virtual machine instances to this bridge when the instances are booted.

To configure a bridge in CentOS 6 or RHEL6, you need to create a file with bridge configuration (for example, ifcfg-brX) and modify the file for the physical interface (for example, ifcfg-ethX).

* Install the bridge-utils package.

`yum install bridge-utils`

* Go to the `/etc/sysconfig/network-scripts` directory:
`cd /etc/sysconfig/network-scripts`

* Open the network script for the device you are adding to the bridge and add your bridge device to it { the file is usually named as ifcfg-eth0 }. The edited file should look similar to the following:
{% highlight bash %}
DEVICE=eth0
HWADDR=00:16:76:D6:C9:45 #change the hardware address 
ONBOOT=yes
BRIDGE=br0
NM_CONTROLLED=no
{% endhighlight %}

*  Create a new network script in the /etc/sysconfig/network-scripts directory called ifcfg-br0 or something similar. The br0 is the name of the bridge, but this can be anything as long as the name of the file is the same as the DEVICE parameter, and the name is specified correctly in the previously created physical interface
configuration (ifcfg-ethX).

{% highlight bash %}
DEVICE=br0
TYPE=Bridge
BOOTPROTO=static
IPADDR=10.0.0.2
NETMASK=255.255.255.0
GATEWAY=10.0.0.1
ONBOOT=yes
{% endhighlight %}

* Stop the NetworkManager
 
`service NetworkManager stop`

* Restart Network

`service network restart`

###Disable the Firewall

If you have existing firewall rules on your hosts, you should disable the firewall in order to install Eucalyptus. **You
should re-enable it after installation.**

Tip: If you do not have a firewall enabled, skip this step.

To disable your firewall:

* Run the command `system-config-firewall-tui`
* Turn off the Enabled check box.

Repeat on each host that will run a Eucalyptus component: Cloud Controller, Walrus, Cluster Controller, Storage Controller, and Node Controllers.


###Configure SELinux

Security-enabled Linux (SELinux) is security feature for Linux that allows you to set access control through policies.
Eucalyptus is not compatible with SELinux. 

To configure SELinux to allow Eucalyptus access:

* Open `/etc/selinux/config` and edit the line `SELINUX=enforcing` to `SELINUX=permissive`.
* Save the file.
* Run the following command:

`setenforce 0`

###Configure NTP
Eucalyptus requires that each machine have the Network Time Protocol (NTP) daemon started and configured to run
automatically on reboot.
To use NTP:

* Install NTP on the machines that will host Eucalyptus components.

`yum install ntp`

* Open the `/etc/ntp.conf` file and add NTP servers, as in the following example.
{% highlight bash %}
server 0.pool.ntp.org
server 1.pool.ntp.org
server 2.pool.ntp.org
{% endhighlight %}

* Save and close the file.
* Configure NTP to run at reboot.
`chkconfig ntpd on`
* Start NTP.
`service ntpd start`
* Synchronize your server.
`ntpdate -u <your_ntp_server>`
* Synchronize your system clock, so that when your system is rebooted, it does not get out of sync.
`hwclock --systohc`
* Repeat on each host that will run a Eucalyptus component.

In case if NTP port is blocked you may run your own ntp server and modify your clients to update from that server.
To set up your own NTP server, follow these steps :
* * *
####Do the following in Main Node ( Only if NTP is blocked ) 

* back up your ntp configuration file
`mv /etc/ntp.conf /etc/ntp.conf.bkp`

* create a new `/etc/ntp.conf` file with the following code in it
{% highlight bash %}
# Use the local clock
server 127.127.1.0 prefer
fudge  127.127.1.0 stratum 10
driftfile /var/lib/ntp/drift
broadcastdelay 0.008

# Give localhost full access rights
restrict 127.0.0.1

# Give machines on our network access to query us
restrict 10.0.0.1 mask 255.255.255.0 nomodify notrap
{% endhighlight %}

* save and close the ntp configuration file
* restart the ntp daemon
`service ntpd restart`

* configure ntpd to start at reboot
`chkconfig ntpd on`
 * * * 
####Do the following in Node Controllers ( Only if NTP is blocked ) 
Follow these steps to instruct the clients to use the local NTP server :

* backup the existing ntp configuration file
`mv /etc/ntp.conf /etc/ntp.conf.bkp`Follow

* create a new ntp configuration file with the following lines in it
{% highlight bash %}
# Point to our network's master time server
server 10.0.0.1

restrict default ignore
restrict 127.0.0.1
restrict 10.0.0.1 mask 255.255.255.255 nomodify notrap noquery

driftfile /var/lib/ntp/drift
{% endhighlight %}

* save and close the ntp configuration file
* stop running the NTP daemon
`service ntpd stop`
* force an update using the local ntp server
`ntpdate -u 10.0.0.1`
* start the ntp daemon
`service ntpd start`
* configure ntpd to start on reboot
`chkconfig ntpd on`
* Synchronize your system clock, so that when your system is rebooted, it does not get out of sync.
`hwclock --systohc`

###Enable IP Forwarding
Edit the `sysctl.conf` on each machine you plan to install the Cluster Controller (CC) component on. IP forwarding is required for the CC to work.
To manually enable IP forwarding:

* Enter the following command on the CC:   
`net.ipv4.ip_forward = 1`     

This ensures that if any dependency (or other unrelated software component) on the system reloads `sysctl.conf` that it won't turn off IP forwarding.



