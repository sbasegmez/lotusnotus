---
authors:
  - serdar

title: "Goodbye my dear: A WAS-Domino implementation will be shutdown..."

slug: goodbye-my-dear-a-was-domino-implementation-will-be-shutdown...

categories:
  - Misc

date: 2010-12-03T17:05:00+02:00

tags:
  - developi
  - domino-dev
  - java
---

In 2003, my customer brought me a very tough problem. They were going to need a content management system for their web sites. Content should be generated and modified by different types of users (from inside company and outside agencies).

First we tried to build a Notes database hosted on a dedicated domino server but massive numbers of impressions were kicking response times underground... But we needed Domino, because content editors were wide-spread inside and outside company. Replication and data flexibility were indispensable.
<!-- more -->
At the right time, IBM announced WebSphere Application Server entitlement for Domino servers. It was limited but enough for us. Accordingly, we were able to use WAS only to connect a domino server and fetch data with NSF data sources. At the presentation layer, however, there was nothing at all.

After days and nights of hardworking, we have developed a complete system with JSP and J2EE classes on top of an NSF database. We built a Java bean library that connects Domino server over DIIOP to get and cache contents from the database. We used tens of JSP pages to build a presentation layer for web sites. Over time, we have hosted 8 different web sites over this implementation. Response times were amazing because of our effective in-memory caching techniques. It was very stable (no restart for months) and secure (no successful DOS attacks over 7 years).

That wasn't enough. A year later, more has been requested. The customer wanted us to gather visitor data and embed on their CRM application. When a visitor fills a form on the web site, we should investigate if s/he is a returning or a new customer and attach the interaction with the brand to his/her contact. Then, we extended our beans to extract the data and submit it to the CRM sub system.

However, in time, world has been changed. Now, social media rules and web sites are more active than they were. Today, an extra microsite is being designed every month. Despite its robustness, performance and scalability, our JSP application is developer oriented. An experienced developer with JSP and Domino knowledge is needed to implement web designs into the site. We tried to use standard HTML coders from creative agencies a couple of times but the result was disaster. Additionally, the company decided to outsource content generation completely. Domino databases became burden this time because when they decide to work with a new agency, we should educate them to use our complex CMS on Notes client.

Last year, we have created a shutdown schedule. Each site has been taken out from the CMS one by one. We redesigned CRM integration with web services and relocated. Half an hour ago, I disabled last module. Next week we will pull the plug...

I learned lots of different techniques from my seven years with this project. Have you ever feel attached to your long-term application projects? I had :)

Looking at the good side: I am using this experience to develop and understand XPages applications. JSP (and JSF) are very different from usual server side programming (comparing to asp, php or agents). If you have a firm understanding of how a Java application works in background, it will be easier to adapt.
