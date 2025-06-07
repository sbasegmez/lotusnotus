---
authors:
  - serdar

title: "Downgrading Domino: XPages may fail..."

slug: downgrading-domino-xpages-may-fail...

categories:
  - Tips & Tricks

date: 2011-02-27T16:29:45+02:00

tags:
  - dojo
  - domino-admin
  - xpages
---

I recently downgraded my domino server from 8.5.2 to 8.5.1 to be able to run Lotus Quickr 8.5.1 which requires older version. This is another issue of 'Dear IBM, please...'

After downgrade, an XPage application failed in some dojo elements like calendar fields. Specifically, I first discovered the problem with missing calendar icon in date picker object.
<!-- more -->
I have analyzed the code, the only difference between the running testing platform and the production was that my Xpage was loading dojo classes from dojo-1.4.3 folder, coming with 8.5.2.

The problem is that when HTTP task runs, even you specified 8.5.1 in XPages compatibility, **it takes the latest Dojo available** . In my server's 'domino/js' folder, there were three dojo versions, namely '**dojo-1.3.2** ', '**dojo-1.3.3** ' and '**dojo-1.4.3** '. Domino 8.5.1 uses dojo-1.3.2 by default. So **I deleted the rest and restarted HTTP task** . Now it's working!

Downgrade process, normally does not remove all codes from the server. So you have to be careful compatibility issues...
