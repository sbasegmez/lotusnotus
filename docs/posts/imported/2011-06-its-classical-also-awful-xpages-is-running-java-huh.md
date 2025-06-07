---
authors:
  - serdar

title: "It's classical, also awful: XPages is running Java huh?"

slug: its-classical-also-awful-xpages-is-running-java-huh

categories:
  - Tips & Tricks

date: 2011-06-25T13:20:59+02:00

tags:
  - domino-admin
  - domino-dev
  - troubleshooting
  - xpages
---

One more classical Java problem! Now on air with XPages development!

I have completed my second Custom Control for OpenNTF Contest. I were packing up everything. I created a blank database, copied all my design elements and set ACL's and other stuff.
<!-- more -->
Ooops; one more unclear error message launching my XPage:

**Error 404**
**HTTP Web Server: Item Not Found Exception**

OK, calm down, I said. I googled and I found it might be related with building. I tried different combinations, restarted my client and server, tried sugar in my coffee, etc. No chance...

The page can be opened in Notes client. The database can be opened in browser but no XPage can be launched over browser.

Awsome huh?

Now it's coming...

I turned on debugging on my HTTP task (tell http debug thread on) and found out the problem:

25.06.2011 14:16: Exception Thrown
com.ibm.designer.runtime.domino.adapter.util.PageNotFoundException: Cannot find the module: **xınvolve100.nsf**
at com.ibm.domino.xsp.module.nsf.NSFService.doServiceInternal(NSFService.java:451)
at com.ibm.domino.xsp.module.nsf.NSFService.doService(NSFService.java:352)
at com.ibm.designer.runtime.domino.adapter.LCDEnvironment.doService(LCDEnvironment.java:304)
at com.ibm.designer.runtime.domino.adapter.LCDEnvironment.service(LCDEnvironment.java:261)
at com.ibm.domino.xsp.bridge.http.engine.XspCmdManager.service(XspCmdManager.java:291)

My PC has Turkish localization and my database is named as "xInvolve100.nsf". My dear Java baby is lowercasing this as "**ı** ".

It's complicated and never been solved by Sun. Yes! The lowercase of "**I** " character is not "**i** ", it's "**ı** ", dotless "**i** "... In my 15 years of professional IT career, it has never been solved.

The lesson of the day:

#### "Never use '**I**' character in your database names. It will fail in Turkey..."

<br />
