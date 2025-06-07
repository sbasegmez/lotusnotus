---
authors:
  - serdar

title: "Need suggestion: Authenticate for HTTP from XPiNC?"

slug: need-suggestion-authenticate-for-http-from-xpinc

categories:
  - Misc

date: 2010-08-25T16:31:09+02:00

tags:
  - blogging
  - domino-dev
  - security
  - xpages
---

I am digging into XPage secrets. There is an XPage containing an IFRAME targeting a restricted resource fro the web server. It may be a view/page/form from another database or file resource under domino\\html folder. No problem viewing this page from browser because you authenticate into web server and can see the resource.

But what about Notes client? From XPiNC, IFrame element calls for a basic web page and of course, it needs authentication!

Another application area of this problem is with client-sided AJAX routines. When you are authenticated with an XPage on Notes Client, a client side HTTPRequest will not be authenticated for another resource on the same server.

Any suggestions?

**UPDATE:**

After Tim's suggestions, I have found a solution and implemented. I also found a bug and solved :)

See the next post...

<http://lotusnotus.com/lotusnotus_en.nsf/dx/http-authentication-from-xpinc-got-help-found-bug-worked-around.htm>
