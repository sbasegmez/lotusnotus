---
authors:
  - serdar

title: "New features of XPages in 8.5.3"

slug: new-features-of-xpages-in-8.5.3

categories:
  - News

date: 2011-05-22T14:45:00+02:00

tags:
  - domino-dev
  - xpages
---

*\[UPDATE: More detailed wrap-up about 8.5.3 is [here](2011-09-blogroll-wrap-up-lotus-notesdomino-8.5.3-is-coming-with-enhancements....md "blogroll-wrap-up-lotus-notesdomino-8.5.3-is-coming-with-enhancements....htm")...\]*

In DNUG, I had chance to chat about XPages...

Version 8.5.3 is coming with great features. Some key points are like that:
<!-- more -->
I talked with Martin Donnelly. He is a great guy and fantastic presenter. He told us that probably, 8.5.3 will not contain a new extension library. However IBM is planning to release a stable version independent from the release stream, using update sites.

XPiNC features will be extended in 8.5.3. XUL will be upgraded to the equivalent of Firefox 3.6.x. A good feature will be the ability to create widgets from XPages directly (w/o composite applications).

There are significant performance improvements in new version. One is preloading. We may preload some XPages applications in both client and server using notes.ini parameters. In addition XSP process will be able to consolidate a number of javascript files in a single request. That will decrease the loading time for XPages (especially at the initial startup phase).

Dojo has been upgraded to 1.5.x version which makes developer able to use mobile controls without any modification/installation. CKEditor is also going to be upgraded which provides help and full screen features.

Some HTML5 attributes are being added into Designer client. Unfortunately we can't use most of them in XPiNC but the implementation of mobility features will be easier with no doubt.

These feature may be changed by IBM, of course and similar blah blah :)

You are probably aware of that GBS has a product named Transformer. It provides direct transformation from Notes-based applications to XPages. As I talk with GBS guys, there is an important investment on improvements in this product. They are working on some large projects right now. I am curiously watching Transformer. Signing [a strategic cooperation agreement](http://www.gbs.com/en/pressreleasedetails/items/ibm-cooperation) between IBM and GBS is a recent development in the global market. BTW, there are jokes around that GBS will also acquire Lotus brand after IBM gives up :)))

We'll see more by June I think. In 8.5.2, IBM lifted confidentiality from beta testers too early. I am expecting the same for 8.5.3 and then, we will see lots of blogs around.
