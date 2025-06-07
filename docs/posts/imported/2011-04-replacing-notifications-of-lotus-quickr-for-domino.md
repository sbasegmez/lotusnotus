---
authors:
  - serdar

title: "Replacing notifications of Lotus Quickr for Domino"

slug: replacing-notifications-of-lotus-quickr-for-domino

categories:
  - Tips & Tricks

date: 2011-04-09T14:22:46+02:00

tags:
  - lotus-quickr
  - xpages
  - domino-dev
---

Today was a spare day between project engagements...

So I decided to deal with the LUGTR web site, especially Quickr issues of the site. There were a problem with notifications. There wasn't notifications in fact! Empty bodies... Later, I figured out that in qpconfig.xml file, the template location has been defined with "\\" instead of "/". Since it is working on SUSE Linux, the engine could not find the templates.

That was easy. I tested some notifications and saw that these will not be useful at all. There are some deficiencies in notification engine of Lotus Quickr for Domino.
<!-- more -->
For instance, although I configure the locale of notification messages as 'tr' (I mean Turkish), month names are still in English (like 4.April.2011). We can customize 'What's New newsletters' content but not subject. In addition an ordinary newsletter seems like that:

```
Your place "Test Place1" has changed in the last 7 day(s), updated last at April 09 2011 12:07:23.

"deneme" has been changed by Serdar Basegmez blah blah...
```

<br />

<br />

Here, you cannot pass a link directly to the content. The user has to enter the index view and find the content by himself/herself (Correct me if I'm wrong)...

Another problem is the membership issue. Normally the user may subscribe/unsubscribe to the newsletter. However, if a group is the member in the place, individuals cannot make such a selection. Moreover, if a person is member of multiple groups which are members in a place, they would receive multiple copies of the same newsletters.

In fact, small teams may not suffer these issues. But regarding larger deployments and especially extranet applications, huge improvements needed I suppose.

My idea is to replace notification engine with another Lotus Domino database. I would like to hear your ideas about this issue. What would you expect from such a database?

If you want to help me about this database, you are also welcomed... It may be an OpenNTF project, who knows :)
