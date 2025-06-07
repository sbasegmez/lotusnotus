---
authors:
  - serdar

title: "Fixed causeless server crashes: Some tips for admins"

slug: fixed-causeless-server-crashes-some-tips-for-admins

categories:
  - Tips & Tricks

date: 2010-08-31T14:42:00+02:00

tags:
  - domino-admin
  - troubleshooting
---

For a couple of weeks, a server in my customer's remote location were crashing intermittently. We couldn't find any specific reason, but we did fix it. How?

I'd like to summarize this incident, in order to give some **tips and tricks** for non-experienced domino administrators about what one can do in these situations...
<!-- more -->
In our case, intermittent server crashes were in random times. Thanks to the "**Automatic Server Recovery** " feature, the server restarts itself and becomes up and operational again. Also, because of **transaction logging** , we experienced neither corruption nor data loss in any crash.

Tip: Enable "Automatically Restart Server After Fault/Crash" feature in server document. Server may crash in the middle of night and users won't be patient until you wake up :)

Tip: Enable "Transaction Logging": This feature increase performance in most cases and provide robustness for your databases.

<br />

I analyzed **IBM_TECH_SUPPORT** folder for **NSD** files. Normally, after a number of crashes, you find some '***harmony*** ' between them. These are some examples:

* **Timing**s of crashes are important. 3-4 crashes at 8:00pm will not be a coincidence. You have to check for scheduled agents, replications, backups or other tasks at that hour.
* NSD files provide information about **which task crashed**(Search for FATAL THREAD). Series of crashes in the same task may be a good start.
* After a crash, NSD provides a **memory dump** (.dmp) for the problematic task. Analyzing this file may give an idea what it was doing just before the crash. A corrupted database may crash your server once with UPDATE task and once with REPLICA.
* Sometimes, the **last line of console** log provides us the crucial information. Several crashes which occur just after mail delivery to a specific user should alert us to check his/her mail database, rule-set, pre-delivery agents, etc.
* Checking for web access logs may also help. In one case, I found that random crashes were originated from 'iNotes name picker'. Because, just before each crash, the last line of web access logs was directing us for the name picker form of inotes database.

<br />

Tip: Ensure that "Run NSD To Collect Diagnostic Information" option has been checked and a reasonably long execution time has been set with "Cleanup Script / NSD Maximum Execution Time" in the server document. The default setting (300 seconds) may not be enough for systems with very large memory.

Tip: Log everything as possible as you can. You don't know when you may need...

<br />

In our case, there were no any 'harmony' between crashes. All incidents were in different times and in different tasks. Even, in a couple of them, NSD timeout was not enough to generate a full dump. So we moved to second step: **technote search** .

We used IBM support resources (Support portal, fixlist database, knowledge bases, etc.) and analyzed different crash cases for out Domino version.There were several similar cases for 8.5.1 FP1. Then we decided to upgrade the server to the fix pack #4. After several days, however, it crashed one more time!

Tip: Generally, maintaining the latest fix pack is a good idea. You may think twice before installing a minor upgrade because they bring some new features you should be prepared for, but fix packs are safe.

<br />

Finally, before creating a PMR, we decided to do some routine night operations on the server.

* We created new replicas of **names.nsf** and **admin4.nsf** databases into a local workstation and replaced with their faulty server copies (when the server is not working, of course), deleted **log.nsf** and **mail(n).box** databases. These databases are always in-use when server is running. According to my experiences, corruptions in these databases are always difficult to debug...
* We deleted **daoscat.nsf** and carry out a '**resync**' operation on DAOS. In early versions of 8.5.x, there are several problems with DAOS operations in technotes.
* We deleted all ".ft" folders in the server to force the server to recreate them. Full-text indexing (especially attachments) is a prevalent issue in server faults.
* We used "**fixup -J**" command on all databases on the server (-J option is for transaction logged databases).
* We used "**updall -R**" command on all databases on the server (-R rebuilds view indexes).

<br />

Two hours of server offline at midnight is enough for these operations. In addition, since fixup and updall tasks, an obvious slowdown perceived till morning. So it is a good idea to schedule these operations to weekend.

Eventually, we fixed. It is 8th day without any crash. We still don't know the reason, but it was more important to prevent it :)

More details are in this great document created by IBM: [Troubleshooting Lotus Domino hangs and crashes](http://www.ibm.com/developerworks/lotus/library/domino-server-crashes/)
