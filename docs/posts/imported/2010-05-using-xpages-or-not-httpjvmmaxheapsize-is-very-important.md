---
authors:
  - serdar

title: "Using XPages or not! HTTPJVMMaxHeapSize is very important!"

slug: using-xpages-or-not-httpjvmmaxheapsize-is-very-important

categories:
  - Tips & Tricks

date: 2010-05-26T11:31:18+02:00

tags:
  - domino-admin
  - java
  - performance
  - troubleshooting
  - xpages
---

Last week, IBM released an important [technote](http://www-01.ibm.com/support/docview.wss?uid=swg21377202)...

System Admins may play with the memory usage of JVM, especially those dealing with limited memory issues. Moreover, as of release 8.5.2, JVM heap size will be decreased by default. It is better to have precautions before upgrade.
<!-- more -->
As of 8.5.1, new notes.ini parameter HTTPJVMMaxHeapSize can be modified. If you are running close to the 32-bit limit of 2GB of memory allocated for the Domino HTTP process, you could experience memory constraints with this value at the default 256MB setting. In prior releases, the default value was 64 MB.

If you are not using XPages on your server, you can decrease this value by setting "HTTPJVMMaxHeapSize=64M" parameter to have a bit more memory.

If you are using XPages, make sure to have 256 MB of heap size, because after 8.5.2 release, 64 MB will be the default setting again. After an upgrade, you may have performance problems on XPages applications.

You need to restart HTTP task after changing this value.

When you are using Java applications like XPages, it means that you'll need lots of memory. I strongly advice using 64-bit operating system, due to the memory addressing limitation in 32-bit systems. In addition, use the latest Domino release (with fix packs) for XPages. Since it is a very new feature, improvements and corrections are continuous. For example, there are huge differences between 8.5 and 8.5.1 releases in terms of performance and scalability.
