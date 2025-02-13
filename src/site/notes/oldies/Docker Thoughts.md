---
{"dg-publish":true,"permalink":"/oldies/docker-thoughts/","dgShowInlineTitle":true,"dgShowToc":true,"dgLinkPreview":true,"created":"2015-08-14"}
---



I am learning Docker, so I am not an "expert". In fact, for those
looking to get started, I just completed the [PyCon 2015 Docker
101](http://pyvideo.org/video/3463/docker-101-introduction-to-docker)Tutorial
and it was great. The exercises are [here](http://docker.atbaker.me/).

That said, have you seen how large the image sizes are? Python 2.7 from
Docker Hub is 747MB on disk. The
[Docker-Django](https://github.com/atbaker/docker-django) image from the
tutorial weighs in at 848MB. This combined with the container starts to
eat disk like nothing I've seen outside of creating VMware vmdk's.
VDMK's, however, are full machines with a proper OS which is readily
configurable while running (see below).

Second, it seems way inefficient to push and pull images around. They
are huge and even with a decent cable ISP it can take a long time. The
fact that you have to push and pull images from a registry sucks quite
frankly.

Third, it is slow. I am also learning Ansible and compared to Docker...
Ansible flat out destroys Docker in performance.

Fourth, I am no master of SysAdmin, but I am learning. If I misconfigure
something in my Ansible playbook, I can go in and fix it via ssh. Can I
simply ssh into a running Docker container and start fiddling if I need
to? I can with Ansible and then make a one-to-one change in my playbook
so its perfect the next time I run it.

Finally, at what point do you stop the containerization? I am learning
this stuff so I can deploy web apps to servers (VPS's). So I rent a VPS
and with docker it goes something like this: Bare Metal -&gt;
OpenVZ/KVM/Etc. -&gt; Ubuntu/CentOS/Debian/CoreOS -&gt; Docker -&gt;
Base images (Ubuntu/CentOS/Debian/CoreOS) -&gt; My app/DB's/Etc. In
other words, containers stacked on containers with more containers
yet! Using a tool like Ansible allows me to cut out another layer of
complexity. Ie. Bare Metal -&gt; Virt OS -&gt; Virt. Machine -&gt; my
apps. What is more, I can use Ansible for Docker when its ready...

Docker is cool tech, no doubt. It is neat to spin up a pre-configured
Gifify server when you don't know anything about NodeJS and run it. The
concept of Docker and my brain are not on the same wavelength, quite
possibly because I am a solo developer and don't have anything that
requires multiple instances of my app running in parallel or whatever.
For now, I am going to stick with Ansible and manual configuration.
