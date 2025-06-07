---
authors:
  - serdar

title: "Blogroll Wrap-up: Lotus Notes/Domino 8.5.3 is coming with Enhancements..."

slug: blogroll-wrap-up-lotus-notesdomino-8.5.3-is-coming-with-enhancements...

categories:
  - Misc

date: 2011-09-28T10:53:50+02:00

tags:
  - blogging
  - domino-admin
  - domino-dev
  - notes-client
  - xpages
---

You would be aware of blogs and tweets about the anouncement of Lotus Notes/Domino 8.5.3 release with great new features. It will be next tuesday (as usual).
<!-- more -->
Last time, I wrapped up some blog posts about 8.5.2. I'll be doing this again and if I forget to give wrong/missing reference for other bloggers, please shout (or comment)...

Before the start, There are three things...

First, usual legal warnings... These features are not promised/guaranteed/etc. and subject to change etc. and based on beta program partners etc. etc. etc. :)

Second, next tuesday will not be the product announcement alone. There will be more. Whole licensing has been rewritten and some changes will also be announced like [express licensing restrictions](http://www.edbrill.com/ebrill/edbrill.nsf/dx/domino-express-restrictions-in-853).

Finally, IBM listened the community and **offering a new product which will be announced next week** . I recently participated a limited call about it and I liked it very much. You should be patient about the details that I won't share but although it will confuse us a bit, it will cover a great gap in the whole product family. \[end of teaser\] :)

Let's start with the wrap-up:

1. [Julian Buss](http://xpageswiki.com/web/youatnotes/wiki-xpages.nsf/dx/Whats_new_in_Domino_8.5.3) started a great wiki about a good summary of new XPages features.

2. [Darren Duke](http://blog.darrenduke.net/darren/ddbz.nsf/dx/domino-and-notes-8.5.3-has-reached-code-drop-5-that-means-the-embago-is-over-so-what-is-new.htm), [Mary Beth Raven](http://www.notesdesignblog.com/NotesDesignBlog/NDBlog.nsf/dx/a-few-of-the-things-coming-in-notes-8.5.3.htm) and [Marie Scott](http://crashtestchix.com/2011/08/04/good-news-new-8-5-3-imap-commandsfeatures/) about the general features:

* Ability to move server based full text indexes to another drive.
* Machine specific policies.
* Purge Interval Replication Control
* Re: and Fwd: are now ignored when you sort by subject
* New mail popup (slide-in alerts).
* Complete control of color of unread email
* Multiple signatures.
* Better control of "Recent Contacts".
* Sametime embedded is now 8.5.1 and Symphony is now 3.0
* A tool is provided to change a single user install to a multi-user install.
* Smart upgrade of Windows Vista and Windows 7
* ID Vault works in Citrix XenApp
* Dropping sessions on IMAP tasks
* Ability to search messages by recipient and subject from the view.

<br />

3. [Declan Lynch](http://www.qtzar.com/blogs/qtzar.nsf/Blog.xsp?entry=1wdacr5ugwf0g), [Eric Brooks](http://www.bleedyellow.com/blogs/erik/entry/8_5_3_app_dev_keeps_moving_forward?lang=en) and [Niklas Heidloff](http://heidloff.net/home.nsf/dx/09272011023837AMNHE9T8.htm) blogged about the new features in Domino Designer 8.5.3

* Source Control Enablement - You can now run DDE with a source control server.
* Designer is MUCH more stable and responsive.
* Designer now has a new "Java Design Element." This allows you to add a java class as a true design element.
* New "XPages" Perspective.
* Better Javascript Editor.
* "Sign" button on all design element lists
* Ability to control automatic popup of infoboxes
* F2 on a design element now allows you to rename the name AND the alias.

<br />

4. [Darren Duke](http://blog.darrenduke.net/darren/ddbz.nsf/dx/domino-and-notes-8.5.3-has-reached-code-drop-5-that-means-the-embago-is-over-so-what-is-new.htm), [Niklas Heidloff](http://heidloff.net/home.nsf/dx/09272011023837AMNHE9T8.htm), [Eric Brooks](http://www.bleedyellow.com/blogs/erik/entry/8_5_3_app_dev_keeps_moving_forward?lang=en), [Julian Buss](http://www.juliusbuss.de/web/youatnotes/blog-jb.nsf/dx/huge-starting-time-improvement-for-xpages-applications-in-the-notes-8.5.3-client.htm), [David Marko](http://blog.tcl-digitrade.com/blogs/tcl-digitrade-blog.nsf/dx/14.07.2011111755DMACWS.htm), [John Jardin](http://jvjardin.wordpress.com/2011/09/12/notesdomino-8-5-3-launch-date-and-xpages-release-notes/) and [Declan Lynch](http://www.qtzar.com/blogs/qtzar.nsf/Blog.xsp?entry=1wdacr5ugwf0g) (also [here](http://www.qtzar.com/blogs/qtzar.nsf/Blog.xsp?entry=mfr6u1708ow0)) blogged about some XPages enhancements:

* Full text sorting
* HTML 5 attributes added to XPages controls
* Preloading XPages applications at client and server startup
* Code aggregation makes JS and CSS files much faster to load
* Dojo 1.6.1
* Much simpler i18n support
* Easier deployment of ExtLib and other OSGI plugins
* Ability to disable tags around some container controls (repeat, panel, etc.)
* Show disabled control for read only
* New Dojo property panel for controls, so that you can set Dojo attributes more easily.
* New performance related application property "Evaluate entire page on refresh".
* New "show XPage instead" property for views.
* In the fileDownload control you can change the image and confirmation message for the "delete" action now.
* A new event "view.postScript()" let's you compute client side JavaScript which is executed every time a full or partial update is executed.
* OneUI version 2.1
* Relational database connection with Ext.Lib.
* (BTW, Extension Library is already compiled for 8.5.3)

<br />

5. [Ulrich Krause (a.k.a. Eknori)](http://www.eknori.de/2011-06-11/lnt-8-5-3-new-widgets-for-android/) and [Paul Mooney](http://www.pmooney.net/2011/06/lotus-traveler-8-5-3-approving-devices-to-access-data/) (and [here](http://www.pmooney.net/2011/08/traveler-whats-coming-with-8-5-3/)) has blogged about Traveler enhancements:

* Lots of enhancements in Android:
* Android has a new set of widgets for mail and calendar.
* Android will have multi-line signatures
* Android Calendar enhancements
* Android Tap to dial for calendar entries
* Android Mail enhancements
* Android OS 3.0 support
* Installation improvements for Lotus Notes Traveler client for Android
* Name Lookup enhancements for Android
* Android chair side calendar actions
* Android 3.0.x tablet menus
* Domino encrypted mail support for Android
* Reply and Forward indicators from Apple devices
* Select which applications are allowed to sync for Apple devices
* New security policy option which requires approval for new devices
* Administrative approval for devices connecting to Traveler
* Symbian\^3 support (with device encryption enforcement)
* Mail routing configuration no longer needed on the Lotus Traveler server
* Meeting notices sent from mobile devices are now sent "from" the mobile device user
* Name Lookup requests are now executed against the user's mail server directory
* Lotus Traveler data only remote wipe for Apple iOS devices
* Domino Mail-in Database lookup
* Group name lookup

<br />

<br />

We may see more in the gold release. Stay tuned :)
