---
authors:
  - serdar

title: "Authenticating Notes users for Web apps automatically..."

slug: authenticating-notes-users-for-web-apps-automatically...

categories:
  - Articles

date: 2011-05-11T17:32:53+02:00

tags:
  - domino-admin
  - domino-dev
  - notes-client
  - open-source
  - security
  - xpages
---

In [one of my previous posts](2010-08-http-authentication-from-xpinc-got-help-found-bug-worked-around.md), I mentioned about a way to authenticate user for a web application inside an iFrame in XPiNC page.

Now I found another solution for this. When a user clicks a link inside a Notes Client application, we may authenticate him on the web server application (either XPages or 'legacy' (!) web application)...
<!-- more -->
This is very important. Because for over 5 years I were searching for such a solution.

Normally, if you are using hybrid application scheme (that is your users are accessing both notes apps and web apps), they need to login to Domino Web server each time. Some companies have problems with password syncronization between Notes password and Internet password and this results in a serious headache!

I know what you think now. By the version 8.5.x, we have SSO with Active directory (SPNEGO). This is a solution. But here are two problems: You need to upgrade your server, configure SPNEGO and only Internet Explorer and certain Firefox versions will be able to use SPNEGO. In addition, some companies have multiple AD domains which makes SPNEGO implementation very difficult.

What may be the purpose of such a tool? One use may be your home page for Lotus Notes users. You may have a portal-like application, listing different applications that user may access. Some applications listed here may be web applications. You may have additional interfaces using XPages for your Notes applications (e.g. reporting) etc.

Before I explain the solution, I want to thank [Tim Tripcony](http://www.timtripcony.com/), because I am using his suggestion... In addition, I am assuming you have read the post above and skipping some technical details about SSO and LTPAToken concepts.

We first develop a redirector agent for general purposes. We may use a web agent (lotusscript) for this. I used an XPage for simplicity. It basically takes two parameters. 'token' parameter takes the hashed LTPAToken string and 'url' parameter is used for target url. Let's see what it does:

```
<?xml version="1.0" encoding="UTF-8"?>
<xp:view xmlns:xp="http://www.ibm.com/xsp/core">

      <xp:this.beforePageLoad><![CDATA[#{javascript:try {

response=facesContext.getExternalContext().getResponse();
token=paramValues.get("token").toString();
rUrl=paramValues.get("url").toString();

response.setHeader("Set-Cookie", "LtpaToken=" + token + "; domain=.developi.info; path=/");
facesContext.getExternalContext().redirect(rUrl)

} catch(e) {
      _dump(e);
}
}]]></xp:this.beforePageLoad>

</xp:view>
```

<br />

<br />

Here, be careful about the domain parameter at the cookie setting. We may lookup this from the server but there is no need to create a lookup cost, so type in manually...

Now, suppose we have an application and we need to send the user to a web application on the same server without login. I generated a form element and an agent to accomplish this. First, let me explain the agent.

We need to create a session token for this implementation. We will use "session.getSessionToken()" method for this. Unfortunately it is not provided in Lotusscript classes. So we will be using a Java agent for this. The code is here:

```
public void NotesMain() {

      try {
              Session session = getSession();
              AgentContext agentContext = session.getAgentContext();
       
              Database db=session.getCurrentDatabase();

              Document doc=agentContext.getDocumentContext();
       
              String token=session.getSessionToken(db.getServer());
       
              try {
                      String token=session.getSessionToken(db.getServer());
                      doc.replaceItemValue("token", token);
              } catch(Exception e) {
                      e.printStackTrace();
                      doc.replaceItemValue("ErrorLog", e.toString());
              }          
       
      } catch(Exception e) {
              e.printStackTrace();
      }
}
```

<br />

<br />

We need to pass the token we created back to the caller. So we are using a NotesDocument here. If anything goes wrong, we will return an error message back.

Now let's look at how we are using:

```
       Dim session As New NotesSession
      Dim ws As New NotesUIWorkspace        
      Dim dbcurrent As NotesDatabase
      Dim agent As NotesAgent
      Dim doc As NotesDocument
     
      Set dbcurrent=session.currentDatabase
      Set agent=dbcurrent.getAgent("TestAuth")
      Set doc=New NotesDocument(dbcurrent)
     
      Call agent.RunWithDocumentContext(doc)
     
      webServer="https://mobile1.developi.info"
      redirector="/test/redirect.nsf/redirect.xsp"
      target="/names.nsf"
     
      targetUrl=webServer+redirector+"?token="+doc.token(0)+"&url="+target
     
      If doc.ErrorLog(0)="" Then
              Call ws.urlopen(targetUrl)                
      Else
              Msgbox doc.ErrorLog(0)        
      End If
```

<br />

<br />

Here '**/test/redirect.nsf/redirect.xsp** ' is the xpages URI we created before. '**TestAuth** ' is the name of the Java agent.

This code can be placed inside a form for testing. You can modify this to use it on anywhere. For example a common method can be created inside a script library.

Now, before finishing, this code will not be running properly :) We should include some warnings...

First of all, no need to say that: You have to use it on multi-server SSO and your configuration should be working properly. This will work in 8.5.2. If you are using 8.5.1, '**RunWithDocumentContext** ' method does not exist. Alternatively you should use a real document (not an in-memory one) and pass it to the agent with NoteID. Remember you should delete the temporary document after you're done.

Another warning is about the bug. I cannot create a bug report yet but as I mentioned on the post above, there are important bugs in this '**getSessionToken** ' method.

1. If you are using Internet Site Documents, getSessionToken does not look up those. I have a workaround here, just duplicate your '**Web SSO Configuration** ' document and clear 'Organization' field on the second one. It seems crazy, but it works! It's important to duplicate it. Creating a second one from scratch will not work because it should contain the same keys...

2. This one is funnier. Your Web SSO configuration document should be named as **LtpaToken** ... Yes you heard it. Using any other name will fail, because it looks up with this name :)

If you don't care these two bugs, you will end up with an error message: "***Single Sign-On configuration is invalid*** " at the java agent.

One word for performance. This java agent is an expensive thing for Notes client. So if you are willing to use multiple times in a short period, you may use a caching algorithm. We don't want to make our users angry :)

Last word, during my tests, I saw '**Your session has expired** ' message. It was not so frequent, so I could not figure out why. That may be related with the token. Token should be properly encoded into the URL.

I don't know you, but this is a very important invention for me. I am so excited to share it. I hope it works...
