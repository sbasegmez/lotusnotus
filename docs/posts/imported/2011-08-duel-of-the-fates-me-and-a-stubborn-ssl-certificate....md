---
authors:
  - serdar

title: "Duel of the Fates: Me and a stubborn SSL Certificate..."

slug: duel-of-the-fates-me-and-a-stubborn-ssl-certificate...

categories:
  - Tips & Tricks

date: 2011-08-18T09:52:00+02:00

tags:
  - domino-admin
  - security
---

Do you procrastinate tasks that you hate? I am...

I had a long-waited task to install a legitimate wildcard SSL for my customer's Domino servers for internal access. More than ten servers and nobody achieved to teach people how to trust home-made SSL certificates...
<!-- more -->
I have worked with SSL certificates in the past and it's an headache. Because SSL shops think that all servers on the world are using IIS :)

First of all, some credits to friends in the yellowverse that helped me with their blogs...

- Ken Yee (got some help from Joe Walters) prepared a FAQ entry: "[Does Domino Support Wildcard SSL Certificates?](http://www.keysolutions.com/NotesFAQ/doeswildcard.html)"
- John Roling (a.k.a. Greyhawk68) blogged: "[Wildcard SSL on Lotus Domino](http://www.greyhawk68.com/greyhawk68/home.nsf/d6plinks/JROG-7KGLCA)"
- Gabriella Davis blogged: "[Moving an IIS SSL certificate to a Domino Keyring File](http://www.turtleweb.com/turtleblog.nsf/dx/11022009232215GDAVGR.htm?opendocument)"

We did the usual stuff. Generated a CSR with "Server Certificate Admin" database and sent to GeoTrust to get a Wildcard SSL certificates. Then installed root and intermediate certificates into the keyring without any problem. However, when I tried installing certificate, I got an interesting error: "Certificate could not be read from file".

Google Gods gave me only two response about this problem, both dated back to 1999. First was resolved as a CSR problem and the other one was unresolved.

I tried everything, including regenerating CSR to receive a new certificate, check CSR-Certificate compatibility, changing language settings, using different machine/notes version/another database etc. I also debugged the merging code with Lotusscript. It fails at an external call to "_dmsecadm" library for viewing information inside the incoming certificate. There were no problem with the certificate or my keyring file.

I created my own Root certificate and signed a test CSR to check if all things working properly. At this point I noticed a difference between certificates.

Usual certificate created in Domino has an MD5-like signature algorithm whereas GeoTrust certificate has "sha1RSA". So I believe it may be the problem. PMR is still open about this issue and I'll update this post if it resolved.

**UPDATE: PMR Result: Lotus Domino Server supports SHA-1 algorithm but Server Certificate Admin database only accept MD5. There is an enhancement request. So before buying SSL certificate, confirm that SSL shop supports MD5 signatures in certificates...**

Finally, Gab's brilliant post gave me an idea. I found iKeyman with GSK5 (coming with the older versions of Websphere). It is the only version that supports keyring files. I opened my keyring file with iKeyman and installed the certificate. I tried the new keyring on the Domino server and it worked!

However, there are still problems. I cannot open this new keyring file on Server Certificate Admin database (it gives invalid keyring file error). I cannot change its password as well, because iKeyman does not support stash files in keyring files. I still don't know what will happen next year when we need to renew this certificate :)

I felt very good when I solved this problem. These are the moments I really enjoy what I'm doing...

Now, it's "**Dear IBM** " time.

I honestly don't get it. Why are we still dealing with keyring files? As long as I know, it's not being used by any other web servers on the market. I don't know much about certifications but I'd like to know if there is a clear advantage to use .kyr files for SSL configuration.

In addition, deploying key files is really difficult on Domino servers. A better solution would be created (like importing them into names.nsf).

Anyway, I created a couple of ideas on [ideajam.net](http://ideajam.net/). Please vote them if you agree...

"[Using more universal SSL certificate stores instead of keyring files](http://ideajam.net/IdeaJam/P/ij.nsf/0/71B481C405E51D6D882578F0002A76CF?OpenDocument)"
"[Using Domino Directory to deploy SSL keyring files](http://ideajam.net/IdeaJam/P/ij.nsf/0/2C052FD0601FB8FA882578F0002AF51D?OpenDocument)"
