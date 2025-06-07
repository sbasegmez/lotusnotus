---
authors:
  - serdar

title: "Multithreaded replication on server?"

slug: multi-thread-replication-on-server

categories:
  - Misc

date: 2010-12-09T15:47:28+02:00

tags:
  - domino-admin
  - troubleshooting
---

Multithreaded replication in Notes client comes with 8.5.2. It's a perfect feature for people like me. I am working 4-5 different servers each time. Formerly, If I had a large database replicating, I were not even able to receive my new messages.

On the other hand, servers have 'pseudo' multithreaded replication. If you set notes.ini parameter "Replicators", there will be more than one replicator task.
<!-- more -->
However I couldn't figure out how it is working. Because the other day we had set up two new servers at one customer and started replication for a couple of very large databases. We have 3 replicators on the hub server.

In the morning, there were calls from users, complaining there are replication problems between locations. I looked at the server and I saw all replicators are trying to replicate the same database with one of new servers. That blocks other scheduled replications. I restarted replica task and a couple of hours later, the same situation occured again.

I think the problem is with the design. Each replicator seems to be assigned to one connection document. So if there are more than one connections to the same server (in our case one connection is for high priorities, another for medium ones and last one for all), it will be useless to have multiple replicators.

More logical approach will be to attach each replicator to servers even there are multiple connection documents.

To sum up the example;

HUB is the hub server. There are SPOKE1, SPOKE2 and SPOKE3 servers grouped in SPOKES server group. Our databases 'test(n).nsf' have high priority replication setting. There are two connection documents (HUB \> SPOKES for High priorities and scheduled at 30 minutes; HUB \> SPOKES for High\&Medium priorities and scheduled at 90 minutes) and 2 replicator tasks.

0 mins: 1st Replicator: HUB \> SPOKE1 for test1.nsf and 2nd Replicator: HUB \> SPOKE2 for test2.nsf

Suppose 2nd replicator finished test2.nsf at 80 minutes. It starts with second connection document to replicate HUB \> SPOKE1 for test1.nsf. It stucks with test1.nsf in this case.

I didn't create a test environment but it seems like that. But the second replicator should be aware of that the first one is also connected to SPOKE1.

Anyone has tested before?

**UPDATE**: I totally slipped up in my example... As Stephan commented, HUB and SPOKE words leads to confusion because normally in a hub-spoke topology, spokes pull, hubs serve... Please suppose we are using a star-topology...
