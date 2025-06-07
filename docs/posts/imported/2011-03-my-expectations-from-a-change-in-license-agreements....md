---
authors:
  - serdar

title: "My expectations from a change in License Agreements..."

slug: my-expectations-from-a-change-in-license-agreements...

categories:
  - Misc

date: 2011-03-28T09:57:00+02:00

tags:
  - licensing
  - business
  - ibm
---

Last week, [Ed Brill](http://www.edbrill.com "Ed Brill") [announced the expected shift in Express licensing](http://www.edbrill.com/ebrill/edbrill.nsf/dx/the-other-shoe-falls-in-a-good-way-for-domino-express). IBM lifted all major limitations (except the user count) from Express Licensing.

In fact, this is a major discount for some companies under 1000 seats.

In his LUGTR 2011 speech, Ed also informed us that license agreement will be simplified as the next release... That was an ongoing issue for sometime as I posted [before](2010-12-did-you-pay-for-lotus-traveler.md "did-you-pay-for-lotus-traveler.htm").
<!-- more -->
I am interested in this issue for a while. One of my customers got some licensing troubles a couple of months ago and I noticed that licensing agreement is not a 'stuff' that you automatically agree before installation. It is a binding contract and means a lot for customer-vendor relation... Although IBM is not using BSA as a puppet to scare companies to sell license (there are companies doing that!), not being compliant in licensing may create unexpected problems for your company!

Anyway, the core issue is that it's complicated and fails to explain some important points. I am listing my expectations from the new agreement, hoping not to be late and waiting for your comments...

I recently [blogged](2010-12-did-you-pay-for-lotus-traveler.md "did-you-pay-for-lotus-traveler.htm") about the need of license if you are using Traveler, Sametime limited or Sametime Community server. There are problems with the Domino Server licensing if you are using 'Domino-embedded' products. Agreements are not so clear about that.

For example, if you have Lotus Quickr for Domino license, it is being used over a Lotus Domino Server. Do you need a separate PVU license for this Domino installation? I think you would not. If you are using Lotus Notes Traveler on a seperate Domino Server, you would need extra licenses. It makes sense since you are paying for Lotus Quickr but not for Traveler. But is the Domino server installed under Lotus Quickr exclusive for Lotus Quickr? I mean if you are using the same Domino installation for Traveler and Quickr?

That seems like a detail but it's important. For a separate Traveler on Domino costs you around 700 USD at a minimum configuration. One may buy one user Quickr for Domino license (license agreement has an unclear expression which I could not understand) to be eligible to use a secondary Domino Server under 100USD... The same discussion is also valid for Sametime limited-standard versions.

At this point, there is another discussion. IBM may prevent an **unreasonable** purchasing transaction as far as I know. You cannot have 25 PVU server license for any product because there is no core fits for it. But **these boundries are not defined in a public document** . Is there a possibility that IBM does not process a single user Quickr license order? That should be clarified!

One more question is related with CEO (Complete Enterprise Option) bundles. I could not find an additional license agreement for CEO bundles but according to the licensing FAQ, CEO bundle provides a user-based licensing for companies and has more extended coverage than express licensing. It seems like Enterprise Agreement of Microsoft.

For CEO Bundles, a company should count every employees (using computer)... Here is a real problem that we could not find an explicit expression in agreements. My customer has over 500 employees. 300 users have Lotus Notes ID's. They are running an operation at a facility and over 100 users are using a POS system. They are using computer, but not Lotus software. Do they count?

Virtualization is another issue in licensing. In fact, almost everyone using VMWare in Lotus Domino infrastructures but nobody cares about licensing. Under certain configurations, you should provide a set of compliance documentation for IBM. There are detailed regulations in Passport site and licensing agreements. However, it is so so so complicated. I think we will see a complete simplification here.

Entitlement Ratio is a relatively new term in licensing. It defines having a certain ratio between resources of the main program and the entitled software. For instance, you have 50 PVU of Lotus Domino Enterprise Server. You are entitled to use DB2 for limited use with the Domino datastore. If the entitlement ratio is defined as 1/1, you will not be able to run DB2 on more than 50 PVU. This ration should be clarified more because it has been defined for user-based licenses as well. There is a problem with comparing apples and oranges here.

That's all for now. I am looking forward for your comments about this topic...
