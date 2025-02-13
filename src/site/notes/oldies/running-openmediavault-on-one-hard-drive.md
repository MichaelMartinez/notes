---
dg-show-inline-title: false
dg-publish: true
dg-home: false
dg-show-toc: false
dg-created: 04/19/2015
dg-updated: 
dg-show-tags: false
dg-link-preview: false
---

Title: Running OpenMediaVault on one Hard Drive
Date: 2015-04-19 14:59
Author: Michael
Category: Hardware
Tags: Linux, NAS, OMV
Slug: running-openmediavault-on-one-hard-drive
Status: published

You want to evaluate or run OpenMediaVault on one hard drive?

No problem, you have two options:

1.  Partition the Disk before you install
2.  Partition the Disk after you install

In either case, use the live CD from
[GParted](http://gparted.org/livecd.php) to work on the disk partitions.
Follow the instructions on the site linked if you have never partitioned
something... It is super easy, you can do it!

Option 1:

Use Gparted to create two partitions; one for the OMV system and one for
your data(media, backups, etc.). Both partitions should be ext4 unless
you know exactly what you are doing, and if that is the case, you
probably don't need to read this. 16GB or more should be sufficient.

When installing, be sure to select the correct (generally smaller)
partition for install.

Option 2:

Boot from the live CD. Your /dev/sda1 where OMV installed will consume
the entire drive. So you need to shrink that drive to a reasonable
level. 16GB or more should do it. Then use the unallocated space to
create an ext4 partition. Reboot and re-login to the web GUI. You now
have a place to put your data...
