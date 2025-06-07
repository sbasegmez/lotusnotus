---
authors:
  - serdar

title: "IBM Connect Session: BP207 - Meet the Java Application Server you already own..."

slug: ibm-connect-session-bp207-meet-the-java-application-server-you-already-own...

categories:
  - Conferences

date: 2013-01-22T12:41:03+02:00

tags:
  - domino-dev
  - dots
  - java
  - speaking
  - ibm-events
---

Last year was my first Lotusphere. I had a great pleasure visiting labs and talking to developers from IBM.

I have met to David Taieb and we were discussing about an idea (or a suggestion) about XPages. We are lacking a (distributed) back-end processing system for XPages. When you have to run a really long process in your application which could be a reporting, calculation or so forth, you don't have anything but to keep users waiting with an hour glass.
<!-- more -->
David sketched a diagram showing how SmartCloud Social Business (a.k.a. LotusLive) handles background administration tasks with an OSGi-based service architecture. I was surprized to hear that they were using a technology that they have already put to OpenNTF: [**DOTS**](http://www.openntf.org/internal/home.nsf/release.xsp?documentIdÃ3A1C5F7ADC513A86257927006B63F1)**- Domino OSGi Tasklet Services** ...

When I return to my office, the first thing I did was downloading DOTS and playing with this new development method. It was fantastic! I was looking at the **Next Generation Domino Agents** . I have also learnt a lot about OSGi development because in contrast to XPages extensibility API, DOTS has a really simple OSGi Context with little dependencies, easy structure etc.

Later, we have developed a powerful [FeedAggregator](https://github.com/OpenNTF/collaborationtoday/tree/master/DOTSFeedMonster/) tasklet for [CollaborationToday.info](http://collaborationtoday.info/) project and I have personally experienced how powerful and fast DOTS is (you may refer to [this post](2012-11-experimenting-dots-task-vs.-java-agent.md "experimenting-dots-task-vs.-java-agent.htm") about the performance of DOTS)

This year, in IBM Connect 2013, We have submitted DOTS as a session proposal with my dear friend Bruce Elgort. As many of you know, it has been accepted. BP207 session will be on January 30th at 10:00 am in SWAN 5-6.

We will talk about Java, OSGi, DOTS, preparing development environment, tips, tricks and some good practices. Our slidedeck includes every single detail to develop and deploy your first tasklet as well.

> Java and in particular OSGi are now very important parts of the IBM Notes/Domino app dev model. In this session, you'll learn about OSGi and how easy it is to extend the capabilities of your Domino server and XPages applications with the Domino OSGi Tasklet Services (DOTS). Whether you want to replace your existing agents with DOTS tasks, or develop new services for your XPages apps utilizing the many Java libraries available - we'll show you how!

<br />

For attendees, there is no need to have any XPages experience but some level of Java knowledge would be needed to understand some concepts better.

See you there!
