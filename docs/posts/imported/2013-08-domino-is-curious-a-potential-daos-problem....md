---
authors:
  - serdar

title: "Domino is Curious: A Potential DAOS Problem..."

slug: domino-is-curious-a-potential-daos-problem...

categories:
  - Tips & Tricks

date: 2013-08-14T16:11:00+02:00

tags:
  - domino-admin
  - troubleshooting
---

After 5 months, here is a new blog post :)

Yesterday, I was upgrading a customer server and I have noticed that DAOS folder was larger than it should be. Since my customer is heavily using attachments in messages, a large DAOS folder is expected but 150 GB attachments for less than 100 users? Something's wrong here.
<!-- more -->
Of course, DAOS wasn't in SYNCHRONIZED state which prevents PRUNE operations.

However, it's more important to find out why DAOS loosing synchronization?

At this point I noticed something very strange.

```
> tell daosmgr databases
14.08.2013 03:04:34   DAOSMGR: Status DATABASES started
14.08.2013 03:04:34   DAOS database status:
...
...
14.08.2013 03:04:34   Database: S:\$RECYCLE.BIN\S-1-5-21-1801674531-1592454029-1177238915-500\$RD39NSB.nsf
14.08.2013 03:04:34   Database state = Synchronized
14.08.2013 03:04:34   Last resynchronized: 14.08.2013 02:40:02
14.08.2013 03:04:34   Ticket count: 239
...
...
```

<br />

<br />

There were lots of similar database records from the recycle bin which is supposed to be empty... So I have looked at that folder via command prompt and yes, those files really exist! After a research, I noticed the problem. Domino is looking for the NSF and NTF files outside the Data folder (even in recycle bin) to be handled by DAOS.

There are two common cases: First, most companies want to keep mail databases after an employee left. Since Domino doesn't provide any mechanism than deleting, they simply move the NSF file to a backup folder at the file level. Second, when needed, they restore a database into the DATA folder and delete them afterwards. In both ways, Domino (and DAOS) keep their track and continue to maintain these databases in DAOS catalog. Consequences?

1. File-level operations will change the status of DAOS to unsynchronized and stop PRUNE operations.
2. NLO files linked to those databases will never expire and you will see lots of unnecessary NLO files filling your precious storage space.
3. Those databases will stay open and locked by Domino processes so cannot be deleted. Worse, recycle bin would not be emptied like in our case: We found 2 GB dead space hidden inside the recycle bin.

Moreover, it's not limited to DAOS-enabled NSF files. "nserver" process will lock all nsf files in your system in case they can be used by DAOS.

Solution for the problem is being more organized for daily operations. Here are my suggestions:

**Archiving mail databases for former employees:**

* If you keep those databases as DAOS-enabled, it doesn't make sense to archive them, because you will not be able to open attachments when needed.
* We have to turn DAOS off and embed all attachments back in the database by "**load compact mail\\xyz.nsf -c -daos off**"
* Then we will change its extension from "**.nsf** " to "**.nsfbackup** " immediately after "**dbcache flush**" command.
* Now we can remove and archive them as you wish.

<br />

**Restoring a database to the server**

* Make sure you have read [this wiki article](http://www-10.lotus.com/ldd/dominowiki.nsf/dx/daos-backup-and-restore)... Read it again... Read carefully one last time. It's important :)
* If you restore a very old database (older than your "Defer object deletion" value in the server config), NLO files linked to the database would be already pruned. So you should use documented methods to find necessary NLO files.
* After restoring the database, be quick. Get your task done and delete the database from the data folder. When deleting, prefer using Administrator client. If you can't, change the file extension before deletion as above.

<br />

**DAOS Catalog State**

* After file system operations, it's possible for DAOS to lose its syncronization state. You can check the state by "**tell daosmgr status**" command.
* To resync, "**tell daosmgr resync**" command will work but remember, it's a CPU-intensive operation.
* Use DDM to monitor DAOS state.

<br />

**General Rules of Thumb**

* Never keep an NSF or NTF file outside of your DATA folder. Put your backup's into compessed files like ZIP or RAR or change their extensions.
* Domino servers should not use file-level antivirus scanners or any other software that may lock files. If you have to use a real time scanner, use exclusions like explained [here](http://www.bleedyellow.com/blogs/lotusnut/entry/dominogofaster?lang=en_gb) and [here](http://www-01.ibm.com/support/docview.wss?uid=swg21417504).
* Always backup DAOS folder :)

<br />

<br />
