---
authors:
  - serdar

title: "Mistakes in Licensing Lotus Products (1)"

slug: mistakes-in-licensing-lotus-products-1

categories:
  - Articles

date: 2010-12-12T17:57:08+02:00

tags:
  - business
  - licensing
  - ibm
---

I were in [Sapanca](http://maps.google.com/maps?f=q&source=s_q&hl=tr&geocode=&q=Sapanca,+T%C3%BCrkiye&sll=40.688969,30.262527&sspn=0.062999,0.133896&ie=UTF8&hq=&hnear=Sapanca/Sakarya,+T%C3%BCrkiye&ll=40.695347,30.263386&spn=0.062993,0.133896&z=13&iwloc=A)for a partner meeting. Thanks to Esra and other local IBMers, we had a great micro weekend despite of the stormy heavy rain :)

As lately discussed, I am facing more and more licensing questions everyday. As I frequently ask them to CEE team, Maciej (Lotus Channel Manager) gave a short presentation about licensing. I hope a customer webcast will be organized soon...
<!-- more -->
I will sum up a list of frequent mistakes of customers and partners. Let's begin...

#### Lotus Domino Designer is free but Deploying application is not...

<br />

If you are using Collaboration Express licensing or one of utility server models, you may use Lotus Domino Enterprise Server to host your applications. But you should have extra "Lotus Domino Enterprise CAL" for all users who deploys custom applications to the server (for every developers to be clarified).

#### How to count 'Authenticated Users'?

<br />

I see that many customers are calculating their users by counting person documents in their domino directory. It's totally wrong. You don't have to acquire licenses for the following cases:

* Your generic administrator account does not count.
* Your test users do not count.
* Departmental user accounts can be created to handle generic e-mail addresses (like sales@domain.com). They are not counted as seperate users

<br />

However, you should have seperate license in these cases:

* Two different persons sharing a single ID (departmental mail box or help desk) should have two seperate CAL.
* Sharing a single id in seperate times (e.g. different shifts) doesn't matters. User means real persons authenticating.
* If you are using a third party application that connects and uses my domino server resources, you'll need an additional license.
* Developing your own auhentication scheme does not help reducing your license needs. For example you developed an extranet application for your customers. They use it anonymously but authenticated via your own code (with session variables). You still have to buy appropriate utility server license for this specific case.

<br />

#### PVU means 50 times the number of processor cores

<br />

Nope! This is the most common mistake to calculate the number of PVU. Processor Value Unit is to calculate server-based licenses proportional to server cores.

The notion of PVU is to create proportional pricing according to the use of processor resources. However, every processors have different performances. So using some processors are more expensive than others. You can use [this link](https://www-112.ibm.com/software/howtobuy/passportadvantage/valueunitcalculator/vucalc.wss) to calculate how much PVU needed for your server.

For example if you are using standard intel-based Xeon processors (3000-3399 or 5000-5499 or 7000-7499 series), each cores will be counted as 50 PVUs. But for a faster Xeon family with 1-2 sockets (e.g. 6500-6599 or 7500-7599 series), each core will be 70 PVUs...

#### I don't need to have a seperate license for my cluster server!

<br />

The answer is: **It depends** ...

If you have an additional server which is not operating (like a backup server), you don't need a seperate license. But an active cluster machine (which is for high availability or load balancing) need additional PVU's. The same rule applies for your test/development servers.

#### I am not using partitioning to keep license costs down

<br />

Believe me, I've seen this :) Partitioning is a techology to use a number of different server images on **the same** machine. You don't have to buy seperate license to use partitioning. If you have enough PVU's, you may run a number of different partitioned servers on the same machine.

Why use partitioned servers? For example you may seperate different zones in your company (departments, business units or brands), create a secured ID vault, create an additional clustered machine, etc.

#### I have express licenses and using Domino server with all features!

<br />

Many customers don't know that express license has some limitations. You cannot use the following features:

* Extended Access Control Lists
* Cascading Directories
* Directory Catalogs
* Directory Assistance
* Central Directory (userless names and address book)

<br />

Most of the express license customers are using directory catalogs and directory assistance features without caring or knowing the limitation.

#### By Domino Enterprise CAL, I am free to setup my Lotus Mobile Connect infrastructure!

<br />

... by buying necessary LMC server licenses. Yes, Enterprise CAL gives you the entitlement for LMC software but only for CAL license. You still need to have PVU-based licenses for LMC server.

#### What if I develop a web application to provide Software as a Service functionality?

<br />

This is mostly valid for Utility Servers. According to the license agreement, you cannot lease your applications hosted on a licensed domino server. You may provide e-commerce services from Domino applications but if you are selling authenticated users for leasing or servicing like in SaaS models, you'll need a seperate licensing model.

You can find more information in Partnerworld site...

#### I don't know how to calculate PVU for my virtual machines

<br />

This is the advertisement for my next blog :) Stay tuned, You'll learn it...

BTW. You may get more information at the following links all the time...

[Notes/Domino Licensing FAQ](http://www.ibm.com/software/lotus/notesanddomino/licensing.html)
[License Agreements](http://www.ibm.com/software/sla/)
[Passport Advantage Agreements](http://www-01.ibm.com/software/howtobuy/passportadvantage/pao_customers.htm)
