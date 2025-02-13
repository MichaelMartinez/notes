---
{"dg-publish":true,"permalink":"/oldies/diy-nas-part-i/","dgShowInlineTitle":true,"dgShowToc":true,"dgLinkPreview":true,"created":"2015-04-19"}
---


I am currently in the process of evaluating several different
[NAS](http://en.wikipedia.org/wiki/Network-attached_storage) operating
systems as part of the back-up
[3-2-1](http://blog.trendmicro.com/trendlabs-security-intelligence/world-backup-day-the-3-2-1-rule/)rule.
I am going to go with a home rolled solution and open source software.
That is, I am going to build the computer from off-the-shelf parts and
use OSS software to run it... mostly.

I have not settled on the specifications for the new machine, but I have
a spare laptop and VMware to test the software. This will be a series of
posts that outline the trials and tribulations of building my home
NAS/server.

The first software I am going to evaluate is called
[OpenMediaVault](http://www.openmediavault.org/) (OMV). It is based on
[Debian](https://www.debian.org/) Wheezy at the time of this writing.
The developer(s) are working on a Debian Jesse version according to the
[OpenMediaVault
forums](http://forums.openmediavault.org/index.php/BoardList/). Truth be
told, I may not attempt to evaluate another NAS operating system as this
system may fit perfectly with what I am trying to do.

Other NAS OS's that may get a shake:

1.  [Amahi](https://www.amahi.org/)
2.  [Xpenology](http://xpenology.com/)
3.  [Nas4free](http://www.nas4free.org/)

The real issue with all of these systems as compared to a Synology or
QNAP unit is cost vs. performance, upgradeability, and what you plan to
do with the system. I admit that I really like the look of Synology's
DSM software. The newest incarnation incorporates Docker technology
which is really, really cool. The lacking point from Synology is the
performance of their hardware. The "bang for your buck" is quite low
compared to QNAP and  even worse compared to rolling your own. Further,
the commercial units are not really upgradeable and are quite expensive.

My plan is to use the NAS for backup of all devices in my house and my
parents, serve media to my HTPC and devices, and provide a play ground.
I also want to learn networking, Linux system administration, and a
level 1 hypervisor like EXSi or KVM. In addition to that, I would like
to continue to learn Python programming for both system management and
the web with Flask/Django.

I will post my trials and tribulations as we go....
