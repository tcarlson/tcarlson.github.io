---
title: How to configure swap for your EC2 instance
layout: post
categories:
  - AWS
  - SysAdmin
tags:
  - aws
  - ec2
  - memory
  - swap
  - sysadmin
---
![Swap](/wp-content/uploads/2015/05/swap.jpg)

Every time I fire up a new EC2 instance, I inevitably forget to configure swap memory for it (<a href="http://www.quora.com/Why-dont-Amazon-EC2-microinstances-come-preconfigured-with-swap" target="_blank">why don&#8217;t they just come pre-configured?</a>) only to realize it later when some application experiences sudden death at the hands of a (shriek!) OutOfMemoryError. I&#8217;ve gotten pretty quick at configuring this, and thought I&#8217;d share the recipe.

The following instructions are for an EC2 instance running <a href="http://aws.amazon.com/amazon-linux-ami/" target="_blank">Amazon Linux</a> (which is basically CentOS). They will probably work for any RedHat-based distro.

First of all, check to see if you already have swap configured:
  
`<br />
$ free<br />
             total       used       free     shared    buffers     cached<br />
Mem:       7697592    7653364      44228        260        988      88732<br />
-/+ buffers/cache:    7563644     133948<br />
Swap:            0          0          0<br />
` 

If the &#8220;Swap&#8221; section is 0, then you obviously do not and you will need to configure it.

Depending on your <a href="http://aws.amazon.com/ec2/instance-types/" target="_blank">instance type</a>, you may or may not have &#8220;instance storage&#8221; (also called &#8220;ephemeral storage&#8221;). If you do, this is an ideal place to have your swap space because it&#8217;s fast, &#8220;close&#8221; to your OS and it gets obliterated if you ever need to stop your instance (so it&#8217;s only good for transient data anyways). 

If you have instance storage available, it should be visible as a storage device in addition to any EBS volumes you have attached:
  
`<br />
$ ls /dev/sd*<br />
/dev/sda1  /dev/sdb  /dev/sdc<br />
` 
  
_(in this case, sda1 is my root (OS) volume, sdb is my data (EBS) volume, and sdc is my instance storage (swap)._

If you do have instance storage and wish to use it as a swap partition,
  
`<br />
mkswap /dev/sdc<br />
swapon -va<br />
` 

If you do not have instance storage (or don&#8217;t want to use the whole thing for swap), you&#8217;ll need to create a swap file instead:
  
`<br />
dd if=/dev/zero of=/var/swapfile bs=1M count=2048<br />
chmod 0600 /var/swapfile<br />
mkswap /var/swapfile<br />
swapon -va<br />
` 
  
_(this means 2048 blocks @ 1Mb/block = 2Gb)_

Then to make sure your swap gets turned on again after reboot, add a line to your `/etc/fstab`:

`/dev/sdc   none     swap    sw     0 0`
   
or
  
`/var/swapfile   none     swap    sw     0 0`

To verify that your swap is now indeed available:
  
`<br />
$ free<br />
             total       used       free     shared    buffers     cached<br />
Mem:       7697592    7653364      44228        260        988      88732<br />
-/+ buffers/cache:    7563644     133948<br />
Swap:        30712        617      30095<br />
` 

In case this didn&#8217;t do the trick, here are the <a href="http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html#InstanceStoreSwapVolumes" target="_blank">official instructions from Amazon</a>.