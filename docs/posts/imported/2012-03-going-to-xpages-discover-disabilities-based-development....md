---
authors:
  - serdar

title: "Going to XPages? Discover 'Disabilities-based' development..."

slug: going-to-xpages-discover-disabilities-based-development...

categories:
  - Tips & Tricks

date: 2012-03-23T11:19:00+02:00

tags:
  - domino-dev
  - xpages
---

Yesterday, I presented at LUGTR XPages Workshop. We have heavily concentrated on transition to XPages development from traditional Domino programming.

I am hearing a very common sentence more and more:
<!-- more -->
"***Damn! I can't even design a very very simple thing on XPages! Because of that, I'm not going to XPages...*** "

There are many reasons that people with traditional Domino application development background fail in XPages. However this "***XPages sucks*** " literature is not even close to those reasons...

I like Notes client development very much. It's easy, fast and robust in many cases. As we always say, some types of applications can be developed under Notes client 90% faster than any other technologies. But let's face the fact that we never speak: Notes client paradigm also prevents us doing so many simple things. Let's give a couple of examples.

We are designing a form for a document. Document has 1-to-N relation to a second document, which is edited by different form. Suppose we are designing a purchase request. This purchase request has a number of items related and we are connecting these items with a unique number which is belongs to the main document. A classical example, right?

Now, we know the fact that A form can be associated with only one document. Sure, you can do some tricks to break this 'disability' but it complicates our development process. Normally, what we do is creating an embedded view and using a 'Dialog' with a subform to create additional items for our purchase request.

While moving this application to XPages, many developers seek ways to do this in the same way they do in Notes. It is more difficult to apply this pattern in XPages (or any web application!). So our developer will complain how much XPages is difficult to make simple things happen!

At this point, the root of the problem should be analyzed. Actually what we are trying to do in XPages is **wrong** ! This worst practice emerged from a disability in Notes client: "You cannot access multiple documents in a single form element...". We have been doing this pattern for so long time that we have accepted this as a right practice.

In contrast, an XPage provides the ability to use as many data sources as you need on the same page. So there are more effective ways to implement such behaviour in XPages.

Let's consider another example. A couple of days ago, a [question](http://stackoverflow.com/questions/9744113/xpages-create-new-copy-of-saved-document-and-open-it-without-saving-it) has been posted on StackOverflow. The question is more complicated than this but the basic problem is easy. You have a view, you are selecting a document, click a button and it makes a copy or new version for the document. Easy for Notes. Your lotusscript code will create a new document, copy necessary fields to your new document and open the new document with UIWorkspace. The new copy has been opened without being saved so the user has chance to cancel.

In XPages, however, this is not possible. You cannot open an unsaved document on-the-fly. XPages can create an on-the-fly document in its context, that's all. XPages cannot do such a simple thing so it must be incapable :)

In fact, what you did in Notes client is the only way to do it because Notes forms, normally, cannot take parameters. If a 'Form' gets a parameter like "copyFrom=UNID" and initialize its fields before opening according to that parameter would be the best way. Because therefore, you may keep all 'creating new stuff' business logic inside your form. Cleaner and more elegant.

Hey! You can do it on XPages! Any XPage can get parameter and better than that 'Data Sources' can have events (which is also bad for Notes. In Notes, forms have events, not documents!).

A last example... It's more related to MVC pattern which means we have to think model, view and controller as different layers in our application. You cannot easily implement MVC pattern on your Notes client paradigm. More precisely, it's always easier not to implement MVC pattern :)

Suppose you have a workflow application in Notes and an 'Approve' button within the form. When the user clicks the approve button, it get's an explanation, ask the approver what to do next (next approver or return the form to the initiator), send an e-mail and change the status of the form. Simple, right? You may put some formula into the button and voila!

In XPages, it's a bit different. Because in your formula, you are not leaving your context in a series of commands. You can lookup what are your options, interact with the user and perform some business logic. It means that you have to go back and forth different contexts (Server and Client) in SSJS code for your XPage. If it's more difficult in XPages, XPages would be incapable, right?

It's totally wrong!

How you handle your data, how you control and how you interact would be seperated in XPages. Your button should do a single thing: Interact. It gets the information about what to interact from the business logic. Modifying your form and sending e-mail is a different story. Did we mention about you are able to define events at the data source level?

So, ask the user what you want to learn, transfer answers to necessary context (scope variable, event parameters, data source, etc.) and trigger the next step for the business logic (e.g. save the data source). The rest would be handled inside your data source. This paradigm will be much more elegant than your implementation in Notes.

**Conclusion**

Moving to XPages is a paradigm shift. We get used to Notes client programming model and its inabilities so much that we are looking ways to implement the same garbage in XPages. But the paradigm shift requires us to redesign the application experience again to be more compatible with the web application model.

We can reuse the data model or business logic. Our lotusscript code would be embedded inside Notes agents or formulas can be converted to SSJS format. But we cannot reuse the application logic.
