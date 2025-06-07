---
authors:
  - serdar

title: "The lesson of the day: Think simple while debugging XPages!"

slug: the-lesson-of-the-day-think-simple-while-debugging-xpages

categories:
  - Tips & Tricks

date: 2011-06-24T08:46:51+02:00

tags:
  - domino-dev
  - troubleshooting
  - xpages
---

Server side javascript in XPages is very difficult to debug. XPages lab is aware of that and try to figure out a solution. OpenNTF has lots of tools and recommendations for debugging.

However, sometimes error messages make me crazy! Here is an example...
<!-- more -->
I am developing an XPages project and I worked on SSJS persistence and scope issues a lot.

Yesterday, in the middle of night, I had a stupid problem in my code.

```
function getDoc(someParams) {
   // ... do something ... //
   return someDoc;
}

function setSomeValue(value) {
  doc=getDoc(someParams);
  doc.replaceItemValue("someField", value);
  doc.computeWithForm(false, false);
  if(! doc.save(false, false)) {
     // ... do some logging stuff ... //
  }
}
```

<br />

<br />

It's a simple code. Get some document and set some fields and save... However, it sometimes throws an error like this:

**Error while executing JavaScript action expression**
**Script interpreter error, line€, col=26: \[TypeError\] Exception occurred calling method NotesDocument.save(boolean, boolean) null**

The error is coming from "doc.save()" method. But the error message is interesting. There is a "TypeError" here. So I put 'print's everywhere in my code to determine if 'doc' object is fine. No solution... It also says null. Again a number of 'print's, no hope! I removed functions and put my code directly into a button, no hope! It doesn't work...

Then I noticed the problem. I was testing the code with anonymous user and **it doesn't have authorization** !!! The error should be "*You are not authorized to do that* ". Or maybe it should not throw an error and just return false (I am not sure about the last one because there wouldn't any chance to know exact problem in this case).

Dealing with high level problems like SSJS persistence and scope issues have prevented me to think simple.

Anyway, the lesson of the day is 'Think simple while debugging. Eliminate the most obvious reasons before ripping your nice code...
