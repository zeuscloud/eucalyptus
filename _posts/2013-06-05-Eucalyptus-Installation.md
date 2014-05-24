---
layout: page
title: "Eucalyptus Installation"
category: doc
date: 2013-06-05 12:00:00
order: 3
---

##Install 

*  Configure the Eucalyptus package repository on each host that will run a Eucalyptus component:

{% highlight bash %}
yum install
http://downloads.eucalyptus.com/software/eucalyptus/3.4/centos/6/x86_64/eucalyptus-release-3.4.noarch.rpm 

{% endhighlight %}

Enter y when prompted to install this package.

* Configure the Euca2ools package repository on each host that will run a Eucalyptus component or Euca2ools:    

{% highlight bash %}
yum install
http://downloads.eucalyptus.com/software/euca2ools/3.0/centos/6/x86_64/euca2ools-release-3.0.noarch.rpm
{% endhighlight %}    

Enter y when prompted to install this package.

* Configure the EPEL package repository on each host that will run a Eucalyptus component or Euca2ools: 

`yum install http://downloads.eucalyptus.com/software/eucalyptus/3.4/centos/6/x86_64/epel-release-6.noarch.rpm`   

Enter y when prompted to install this package.

* Configure the ELRepo repository on Main Node:

`yum install http://downloads.eucalyptus.com/software/eucalyptus/3.4/centos/6/x86_64/elrepo-release-6.noarch.rpm`    

Enter y when prompted to install this package.

* Install the Eucalyptus node controller software on each planned NC host:

`yum install eucalyptus-nc`

* Check that the KVM device node has proper permissions.
Run the following command:   

`ls -l /dev/kvm`

Verify the output shows that the device node is owned by user root and group kvm.   

`crw-rw-rw- 1 root kvm 10, 232 Nov 30 10:27 /dev/kvm`

If your kvm device node does not have proper permissions, you need to reboot your NC host.

* Install the Eucalyptus cloud controller software on Main Node:

`yum install eucalyptus-cloud`

* Install the Eucalyptus cloud controller software on main node:

`yum install eucalyptus-cloud`

* Install the software for the remaining Eucalyptus components in main node.

`yum install eucalyptus-cc eucalyptus-sc eucalyptus-walrus`


