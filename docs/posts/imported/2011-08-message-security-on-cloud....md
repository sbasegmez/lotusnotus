---
authors:
  - serdar

title: "Message Security on Cloud..."

slug: message-security-on-cloud...

categories:
  - Misc

date: 2011-08-07T19:40:00+02:00

tags:
  - cloud
  - domino-admin
  - security
---

Using cloud-based services for message security makes sense...

The problem is simple. You have a mail server, you simply put an Anti-spam/Anti-virus software/appliance on your DMZ to receive your messages at the edge of your network, checks them for spam and virus risks and permit clean messages only.
<!-- more -->
I rather prefer moving the edge system out to the network. You simply change your MX records, a service provider receives your messages, scans them and transfers clean messages to your mail server. There are obvious advantages here:

- Not dealing with any software or appliance (administration, upgrades, management, usual stuff),
- Avoid costs and pay as you use, mostly per user base,
- Dirty messages don't consume your valuable bandwidth,
- If your network goes down, cloud will continue to receive your messages.
- Some providers also offer archiving solutions which lets you store all of your messages in the cloud.

In addition, if the cloud service includes outbound messaging support, I mean, if you can send your messages by 'relaying' them to the service provider, it will be more useful. You may centrally enforce some compliance rules, get rid of star topology for your distributed servers, avoid dealing with blacklisting issues, etc.

For small companies, these services are life-saver. For large companies it may provide serious cost-reductions as well.

I am using this architecture for years. Since I'm using cheap colocation services, it was a real problem for me to deal with blacklists, downtimes, etc. First, used dyndns.org services. But their support failed to solve my issues. As Google acquired Postini, I switched.

A couple of years ago, one of my customers moved its messaging security to Postini, currently happy with it. At the end of the day, they are spending much lower budget than maintaining a Trend Micro gateway server.

So why we have chosen postini?

The most import thing is its ridiculously low price! You heard right. It's seriously cheap. We tried benchmarking Postini with its rivals for my customer and there were an unbelievable price difference. Some providers (like MessageLabs, acquired by Symantec later) denied trial, for instance, under 2K seats, while Google is simply open to single user trials. Some doesn't have any presence in Turkey.

Postini is not a premium service and we experience some difficulties in such a low price. For example, there are some problems in DKIM support. You may not get satisfactory answers about some mysteriously-spam-marked messages. The service is not working as 'store \& forward', so there are lags if your server goes down. But you can live with it.

As much as I know, Symantec, Microsoft, McAfee, ScanSafe, Websense and Webroot are other alternative providers for hosted message security.

I would like to know if there are any opinions on those services and comments against hosted security.
