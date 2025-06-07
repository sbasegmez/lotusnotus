---
authors:
  - serdar

title: "Test platform for IBM Connections..."

slug: test-platform-for-ibm-connections...

categories:
  - Misc

date: 2012-09-07T11:52:43+02:00

tags:
  - best-practices
  - domino-admin
  - lotus-connections
  - pov
  - wishlist
---

I've been in UKLUG lately. I heard lots of discussions about test platform for IBM Connections.

People who wants to implement new deployments, develop and test widgets or customize user interface, all need to install Connections.

So I just wanted to blog about my test platform. I'm currently doing some Connections business and my customers are expecting UI customization, error trapping, configuration changes, etc. Therefore, I have installed a full Connections image on my computer.
<!-- more -->
I am using Thinkpad T420, i5 2.4 Ghz CPU, 8 GB RAM, Windows 7 Professional installed, one SSD (for windows and apps) and another SATA disk for data and VM images.

There is a Windows 2008 64 bit image on VMWare Player. 2 processor assigned with 4704MB RAM in this image. I have found this memory level by trial and error. Less memory kills the VM and more slows down my computer. As LDAP resource, I'm using a Domino server on my computer which I'm also using for XPages development. this configuration is working smoothly. Start, stop and restart times for Connections are acceptable.

Configuring networks is a bit tricky. I am using a NAT to provide internet connection for the VM and host-only connection to provide connectivity between my computer and the VM.

The only problem is that I have used Windows 2008 for this setup. You can use it for evaluation purposes around 180 days (you can extend standard evaluation time for a couple of times). It has been expired last week. It's still working with a black screen, but sure it's not legal to use it anymore. Therefore I have to reinstall it on a Linux image now. Linux will probably consume much less memory, so I strongly suggest using Linux.

Of course, you have to consider other legal things like test period for IBM Connections or installing Domino on your local workstation etc.

Currently, I'm using the test server for pretty much everything and quite happy about its performance. Alternatively, Niklas is [providing a Connection image](http://heidloff.net/home.nsf/dx/02.08.2012081928NHE9F9.htm) to be used on OpenNTF projects. You can also use IBM cloud environment for a little payment, as a test platform.

Hope this helps.
