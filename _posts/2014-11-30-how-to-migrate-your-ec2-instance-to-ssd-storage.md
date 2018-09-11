---
title: How to migrate your EC2 instance to SSD storage
layout: post
categories:
  - AWS
  - SysAdmin
---
![SSD](/wp-content/uploads/2015/05/HDD-VS-SSD-825x429.jpg)

Amazon Web Services <a href="https://aws.amazon.com/blogs/aws/new-ssd-backed-elastic-block-storage/" target="_blank">has made SSD the default storage type</a> for its EBS volumes and it&#8217;s actually <a href="http://aws.amazon.com/ebs/pricing/" target="_blank">pretty cheap</a>. However, to take advantage of this, you'll need to migrate your production storage volumes to SSD. 

<!--more-->

I had been putting this off for awhile but it went surprisingly smoothly. The steps I took:

  * Create snapshots of all (magnetic) EBS volumes
  * Create new (SSD) volumes from those snapshots in the same availability zone
  * Stop the EC2 instance
  * Detach the old (magnetic) volumes from the instance
  * Attach the new (SSD) volumes to the instance, making sure to use the **same device names** as the original volumes (e.g., /dev/sda1, /dev/sdb)
  * Start the EC2 instance
  * <a href="index.php/2015/02/22/how-to-configure-swap-for-your-ec2-instance/" target="_blank">Reconfigure swap</a> (mine disappeared after the stop/start for some reason)

You should be good-to-go at this point, the only gotchas for me were that `/dev/sda` is **not** the same as `/dev/sda1` and that my swap partition disappeared as a side effect. Our total downtime was less than 5 minutes, not bad!