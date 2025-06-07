---
authors:
  - serdar

title: "XPages problems after 8.5.2 upgrade!"

slug: xpages-problems-after-8.5.2-upgrade

categories:
  - Tips & Tricks

date: 2010-09-29T12:34:22+02:00

tags:
  - domino-admin
  - domino-dev
  - troubleshooting
  - xpages
---

Last week, I posted about the fastest upgrade of my life. Murphy's law worked again... "***A working program is one that has only unobserved bugs*** ."

Now there are two serious problems with XPages applications that were designed for 8.5.1...
<!-- more -->
The first problem is my newsletter subscription application. I have designed this application a couple of months ago but didn't move it to production. This morning, I decided to launch it but it didn't work with an error: "**HTTP Web Server: Command Not Handled Exception** "

After some digging, I have found another designer error "ClassCastException" when opening Xpage designs. The problem was with SimpleActions classes and I tried to convert simple actions in my code to javascript. That prevented the designer error.

But I were still getting the page error. I removed some parts one by one to find the exact source of the error. Eventually, I found the problem at the language custom control.

context.setLocaleString("tr");
context.reloadPage();

<br />

If I remove reloadPage() part, the page will be working. Google found nothing...

The second error is bigger and more interesting. There is an XPages application working on R8.5.1. Now it's not working on 8.5.2.

Interesting point is that if we save and build an XPage on server, it does not work. When we build it on local and copy into the server replica, it works again! I am now trying to trap the exact error. But it seems there is an issue with compile and build processes. We tried everything (clean, build, build all, different clients, differen servers, etc.)

Again, Google found nothing about it...

I'll keep this post updated about the situation.
