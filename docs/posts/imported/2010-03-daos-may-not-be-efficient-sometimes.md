---
authors:
  - serdar

title: "DAOS may not be efficient sometimes"

slug: daos-may-not-be-efficient-sometimes

categories:
  - Tips & Tricks

date: 2010-03-31T11:13:07+02:00

tags:
  - domino-admin
  - troubleshooting
---

Always saying that we will have disk space savings around 30% using DAOS. But in some specific cases, the feature causes remarkable extra disk usage.

About a month ago, we have implemented DAOS for a customer with 6-7 servers. Yesterday, we have been surprised analyzing some servers to see how much space gained by DAOS feature.
<!-- more -->
For one messaging server, 137 users have 36 GB of logical size (without DAOS) and 15 GB of physical size (actual NSF size). DAOS folder has 9 GB of attachment storage. Comparing 24GB to 36 GB, we have a saving of 33% in mail databases, which is an expected situation. On the other side, hub server has 25 GB of attachment in DAOS folder which results in 90% growth for only 20 users.

This hub server is at the center of customer's distributed topology. It also routes whole SMTP inbound traffic from the security gateway to other servers. Since mail.box databases are 'DAOS-enabled', every attachments coming from internet are stored in DAOS repository. Although 90% of these messages are deleted upon transfer, NLO files are stored for a number of days defined in 'deferred deletion interval'. The more you define this interval time, the more DAOS folder grows and inefficiency increases.

Most of the cases, during DAOS implementations, we decide the value of 'deferred deletion interval' according to the restore operation frequency of the customer. This duration, as you know, is the number of days that DAOS keeps unreferenced NLO files. Too high values results in unnecessary usage of disk space whereas, low numbers make the restore procedure more difficult. For example, if your deletion interval is 30 days and one user wants to restore a message deleted 40 days ago, you should restore the database first, find the name of NLO file and restore it among thousands of files. If you are not using full-backup everyday, you should also consider your backup frequency determining the deletion interval.

In another customer, we have enabled DAOS for a large deployment, but didn't transfer all mail files to DAOS repository. In this case, several mail databases and mail boxes created tens of gigabytes attachments in DAOS repository. In a server with large number of mail databases, mail.box database will cause an unnecessary duplication of mail attachments if you do not implement DAOS for all mail databases.

My suggestions for system administrators are;

* If you are using distributed locations, use a messaging gateway server at the entrance point of messages and do not use DAOS in that server. This topologic approach may also be useful if you are using antivirus software or mail archiving system. If you need to enable DAOS in the gateway server, activating DAOS for mail(n).box databases is not a good idea, in spite of IBM's recommendation. IBM Support suggests that enabling DAOS for mail(n).box databases decreases disk I/O for high volume of message delivery. But you'll have to choose between some performance and disk space in this case.
* If you enable DAOS for your server and mail.box databases, do not lose time to activate DAOS for other mail databases. Otherwise, considerable amount of attachments will be duplicated depending on your message traffic.
* You should use 'Deferred deletion interval' parameter very carefully. If your restore frequency is low, 30 days or lower may be used confidently.
* The disk space utilization with the use of DAOS depends on two factors. Firstly, employees' working habits and company's document management system (or the use of file server) affects the attachment load on message traffic. The second factor is the number of users. While you may see 40-50% of disk space utilization for a large deployment of 500-1000 users; one may not tolerate the management cost (complexity, time and effort) against just 5% of disk space saving for a system with 10-15 users.

<br />

I welcome your comments for different cases and experiences...
