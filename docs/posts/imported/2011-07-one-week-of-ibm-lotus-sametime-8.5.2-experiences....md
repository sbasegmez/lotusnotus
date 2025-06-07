---
authors:
  - serdar

title: "One week of IBM (Lotus) Sametime 8.5.2 Experiences..."

slug: one-week-of-ibm-lotus-sametime-8.5.2-experiences...

categories:
  - Tips & Tricks

date: 2011-07-31T21:21:00+02:00

tags:
  - best-practices
  - lotus-sametime
  - social-software
---

Last week I had a couple of Sametime 8.5.2 implementations, one upgrade and another from zero... I'd like to share my experiences...
<!-- more -->
First of all, products working on Websphere platform like Sametime, Quickr and Connections needs extreme caution in installation and upgrades...

The golden tip is **in the book** ! Follow the [guide](http://www-10.lotus.com/ldd/stwiki.nsf/dx/Installing_on_AIX_Linux_Solaris_and_Windows_st852) and in any suspect, ask for recomendation. Write down every anomalies you have seen. There are some additional resources which should be reading before start:

My fellow champion [Gabriella Davis](http://blog.turtleweb.com/) posted "[Sametime 8.5.2 - Install Cheat Sheet and Forget-Me-Nots](http://www.turtleweb.com/turtleblog.nsf/dx/07062011112426GDAE6P.htm)".
Classical wiki article is very helpful even though it's for 8.5.1: "[Installation and Setup of IBM Lotus Sametime 8.5.1 - From Zero to Hero - The next Generation](http://www-10.lotus.com/ldd/stwiki.nsf/dx/Sametime_8.5.1-From_Zero_to_Hero-Next_Generation)"
Read release notes (credits for [Paul Mooney](http://www.pmooney.net/) and his RTFR rule...): "[Release notes - IBM Sametime 8.5.2](http://public.dhe.ibm.com/software/dw/lotus/sametime/st852/rnST852.html)"

**Wait for nothing part...**
My first tip will be that; use your time efficiently! When you install any WAS-based component for the first time (system console in our case), it takes hours. You are just waiting for it... It is similar for the upgrade case. So, my suggestion is to start installing/upgrading system console as soon as possible. In the mean time, I can suggest some tasks:

- Eat (!)
- Prepare other installation packs (moving them into the particular machine is a good idea).
- Register and install Domino server.
- Prepare your start/stop scripts (if you are not going to use services).
- Have your meetings (admins, firewall guys)
- Configure servers, firewalls, test users and clients etc.

**Lots of errors to deal with...**
Look at the installation logs after each step. There may be some little errors. Write down those errors in case of a possible PMR step in the future.

In our case, an error popped up while installing Media Manager: "Not enough storage is available to process this command". It was a Windows 2008 error and probably related with memory. We changed paging file settings but this time installer could not start and it was throwing something like "undocumented error". We tried removing profile directory of Media Manager and it worked... In case you need :)

**Configuration issues...**
In my previous installations, I were using Cell modes for each component. This time I used 'Primary Node' installation. Cell mode complicates start/stop methods because of multiple deployment managers. However, if you are using this approach, remember that configuration files are hold and pushed by Deployment manager even for different physical machines.

For example to enable NAT-Traversal mode, you have to modify stavconfig.xml file. If you modify it on the Media server, it will be reset after restart. You have to use the one in deployment manager (System console server, because it's the first server you installed).

Before starting, be specific about what you need. So you can change many things in configuration at once. Because many changes need restart and it takes long.

**NAT / Network issues...**
TURN can work with UDP or TCP modes (enabling both results in some problems and is not adviced). Remember that some firewalls or routers may have issues with UDP. My customer had this problem and we had to use TCP for TURN server.

For troubleshooting process, you need to know that TURN server is not checked by any tasks. If it's not working, users cannot see audio and video, that's all. If NAT-Traversal mode is not activated, you will not see 'Call computer' options on clients outside firewall. After activating this setting, clients will be able to call each other but A/V data should be flowed over TURN server.

During our tests, we had very interesting situation. Inside firewall, A/V was working fine. But clients outside firewall could not hear audio and see videos on one-to-one calls although multiple calls and meeting A/V were working. Later we noticed the problem. If you enable NAT Traversal communication, clients should be 8.5.2, otherwise they don't know they will use TURN server on A/V calls.

Use a single domain name for all addressing and make sure it can be resolved inside and outside your network without any problem.

**Use a clean sheet everytime...**
My fresh install implementation had a complicated history. Another guy tried installing for several times and he messed up everything. I tried cleaning everything before starting. However there were still some dirty stuff which created problems for me.

For example, when you install Community Server for the first time, it changes some configurations on your server. But it doesn't touch some configured parts. In my case, there were lots of problems on authentication. I noticed that the guy before me couldn't configure LtpaToken and community server did not edit my existing SSO configuration. There is a good page in the documentation that explains all checks you should do on Domino configuration: "[Verifying the Lotus Domino server document settings](http://www-10.lotus.com/ldd/stwiki.nsf/dx/Verifying_the_Lotus_Domino_server_document_settings_st852)".

**Be systematic...**
Use a system. Keep one browser open for the guide (Only one window is enough to prevent confusion and distractions. Never use multitasking!) , one for technotes (I usually prepare a large collection of related technotes about what may go wrong) and another for forum/google searchs.

I use a note-taking application (like ActionOutline) to keep track of what I'm doing. I record passwords, IP numbers, server names, error messages I saw, etc. For example a full page for all url's is a very good idea. Because, each time you need to connect meeting server's WAS console, for instance, it may be time consuming to search its port from the configuration. If you know you will do the next upgrade for this server, write down a full list of your changes for your installation.

That's all for now... I would update this as I remember more tricks... No need to say, of course, you may contribute by commenting :)
