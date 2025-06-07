---
authors:
  - serdar

title: "For dummies: Lotus Quickr 8.5 for Domino"

slug: for-dummies-lotus-quickr-8.5-for-domino

categories:
  - Articles

date: 2010-12-24T18:44:04+02:00

tags:
  - lotus-quickr
---

While dealing with the conference, I didn't posted any useful entry to my blog :)

Nowadays, I am working on Lotus Quickr 8.5 for Domino and Sametime 8.5. We'll review Sametime another time but I wanted to mention about Quickr with fresh experience. As we are moving our user group site to Quickr and I have been certified recently, I am ready for questions...
<!-- more -->
**What is Lotus Quickr and What is it good for?**
Lotus Quickr, basically, provides a consistent web 2.0 interface for team collaboration. As a social software, it contains very easy-to-use UI, learnable structure and some vertical tools (commenting, sharing, etc). Although the main use may be seen as document management, we may generalize the product as collaborative content management application.

Quickr has content repositories named as 'place'. Contents can be anything such as url, task, calendar entry, document(s), form data, list or HTM/rich-text entries. Places provide us different views for those contents.

Using Quickr, a team can manage their documents, use a common calendar, configure discussion forums, create dynamic lists and designate simple approval flows.

**What is 'for Domino' and 'for Portal'?**
This is the tough one :)

As you may know, Lotus Quickr is the successor product of Lotus Quickplace. After the version 8.0, IBM developed dual version, for Domino and Portal platforms (J2EE). To be frank, we supposed it was a closure for the Domino version and IBM would continue with the Portal platform. Moreover, we have even chosen Portal environment for the user group site because of this.

However, Domino version has been evolved better than Portal version. Furthermore, IBM dropped the horizontal navigation in the Portal edition and turned back to the vertical model in the recent portal version. It means that Domino version will not be orphan in spite of our concerns.

Officially, you are recommended to use the platform you know better. But I have to warn you about differences:

First of all, document management infrastructures are very different. Portal version is using Java-based WCM (Web Content Management). Obviously, it has more flexibility than Domino. Although Domino version improved much, WCM has more capabilities like complex metadata. For instance, if you are using Domino version, it is not easy to use explicit implicit versioning for ordinary documents. Domino enables explicit implicit versioning with custom form data. Custom forms provide ability to define contents with metadata and attach file(s) inside ([sample video](http://www-10.lotus.com/ldd/lqwiki.nsf/xsp/.ibmmodres/domino/OpenAttachment/ldd/lqwiki.nsf/F1D9ECD5BB4BF4968525779D00727DBE/attach/QD%208.5%20-%20Creating%20Custom%20Forms.mp4)).

ECM (Enterprise Content Management) integration was not available in Domino version before 8.5 version. But if you are using FileNet, you may define ECM connections thereafter. This is important for customers (such as finance industry) with large number of documents and high-volume of transactions. Because FileNet has a weak UI and actually, Quickr has been positioned to cover this deficiency.

In Domino version, documents are stored in NSF databases. You may get important advantages from NSF, because it supports DAOS and replication. It is an unknown issue for me to customize and utilize custom views and folders. So far I didn't deal with this issue.

On the other hand, Portal version stores contents on WCM. Even though it has an old, slow and complex structure, WCM provides important flexibility in presentation and authoring level. However, you need to rule JSP. I tried some customization here and frankly, it is very complex.

In theory, you may use portlets in Portal version, whereas we don't know many application extension in Domino side (for 8.5) yet. By the way, you should consider licensing issues while using portlets because in the licensing view, Portal functionality can be utilized only for Quickr.

Finally, Domino offers 'Offline Places' support (as of 8.5.1) and PlaceBots (running agents on contents).

**What about Free Quickr Entry?**
We no longer recommend Quickr Entry. It has been stopped at the version 8.2. As of 8.5, you cannot install entry places and connector cannot use entry places! As I [blogged](2010-11-good-news-lotus-quickr-entry-has-been-history....md) before, better alternative is on the way...

Still curious? Lotus Quickr Entry provides personal places where you may share your documents with other users. You upload your files and send links to others. But it has limited security features. Although you may hide your files in your place, it does not provide an access control. Anyone has a link can access the document you shared. So it is not a personal document management tool. Instead, it replaces 'SHARE' folders in your file server.

**What is Quickr Connector?**
Quickr Connector is an additional plugin to access your contents in Quickr server. It provides an interface working with Windows Explorer, Lotus Notes, Lotus Symphony, Outlook, MS Office and Lotus Sametime.

Connector is a cruicial part of Quickr. You may upload your excel document to a place directly within MS Excel. You can access older versions, revise current ones from windows explorer. When you send a large file via E-mail, Quickr may automatically upload the document into the server and replace a URL in the place of attachment.

**So What is new with Lotus Quickr 8.5?**
Lotus Quickr 8.5 for Domino came with very important improvements. To sum up;

* Multilanguage support: Before Quickr for Domino 8.5, you were installing another server to support a second language. Now, it detects your language and present different UI.
* Dynamic Lists are my favorite. You have to watch [demo video](http://www-10.lotus.com/ldd/lqwiki.nsf/xsp/.ibmmodres/domino/OpenAttachment/ldd/lqwiki.nsf/F1D9ECD5BB4BF4968525779D00727DBE/attach/QD%208.5%20-%20Lists.mp4).
* 'What's New' page and feed is available now. Therefore, if you didn't enter the place for a couple of day, you would take a look what's going on.
* You can embed Lotus Quickr calendar into Notes calendar.
* A fresh new rich text editor provides pasting rich text from MS Word or Symphony.
* No need to install ActiveX anymore. It was a pain in the ass.
* We have trash folder as of 8.5.
* Ability to define ACLs at folder-level.
* ECM integration for FileNet and IBM Content Manager
* SUSE Linux support for Quickr Servers

<br />

**How can I try?**
Best way to try IBM products: [Greenhouse](http://greenhouse.lotus.com/).

It is easy to install Quickr. It takes 4-5 hours for Domino and 7-8 hours for Portal. But Greenhouse is ready 7/24.

**Still want to try at home, what is tricky?**

* Use LDAP. It will be easy to configure SSO and Sametime integration.
* Use a seperate server and even different domino domain. There are several reasons. For example, you may stick with a version on a production server. Because current version 8.5.1 does not work with Lotus Domino 8.5.2. It alters your web site settings. You cannot use your designated login form. UTF8 encoding is a must. If you already use LDAP, you don't need your public address book in this seperate server. As a best practice, to avoid certification issues, use your current certifier and admin id's to install 'First Standalone' domino server.
* There are limitations with the number of certifiers, beware!
* This 'Login Form' issue is crucial. When you use Multi-server SSO, you have to define a custom login form at 'domcfg.nsf' as you know. Lotus Quickr may accept non-ldap users (admin user or local members for places). So you have to use Quickr's own login form. Otherwise, you cannot log in as an administrator at first.
* The first Admin user you have created during installation should not be listed in LDAP.
* File upload has been limited with 10MB at first. To increase, you should configure two different settings, one in Domino, another in Quickr.
* Scalability is important. Ordinary Quickr setup allows up to 900 members for each places. The reason behind this limitation, it stores member records in the ACL note. If you configure Expanded Membership Model (EMM), this limit increases up to 4000 members. But careful here, there are different [considerations](http://www-10.lotus.com/ldd/lqwiki.nsf/dx/Expanded_membership_qd85) for EMM.

<br />

So far it is all I can say about Lotus Quickr 8.5 for Domino. A couple of last-minute-notes: Documentation has been moved to [wikis](http://www-10.lotus.com/ldd/lqwiki.nsf/xpViewCategories.xsp?lookupName=Lotus%20Quickr%208.5%20for%20Domino%20documentation) instead of InfoCenter. There are [useful videos](http://www-10.lotus.com/ldd/lqwiki.nsf/dx/Quickr_8.5_Videos) in wikis.

Finally, are there any more questions?
