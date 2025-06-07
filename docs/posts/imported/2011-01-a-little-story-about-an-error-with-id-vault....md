---
authors:
  - serdar

title: "A little story about an error with ID Vault..."

slug: a-little-story-about-an-error-with-id-vault...

categories:
  - Tips & Tricks

date: 2011-01-12T16:17:59+02:00

tags:
  - domino-admin
  - troubleshooting
---

I like blogging such stories about strange incidents in customers :) I hope it helps my newbie readers to learn how to troubleshoot these kind of problems.
<!-- more -->
Today I made a short visit to a customer to chat. A couple of weeks ago, we struggled with some SSO issues between Domino and Portal. It seems that 'Reset Password' feature of ID Vault was not working since SSO operation. I told them there is absolutely no relation between two issues. I were so sure :)

Last year, the most popular topic in my blog was the one related to ID Vault problems. The reason is obvious. There are some 'nice to have' features that people try out implementation but in case of failure, they may just give up. However, ID Vault is an extremely useful tool that cannot be avoided...

I assumed this to be a classical issue, as well. I checked everything, from public keys to ACLs but it was OK. I recreated cross certifications, tried different users, etc. Nope! ID Vault was giving the classical error: "**Missing or invalid Password Reset Trust certificate from 'XXX' to 'YYY': Note Item not found** ".

Here, I noticed a difference. The classical error may end with '**Entry not found in Index** '. Because, normally, the problem originates from a missing certificate document.

So I opened some debug parameters (Debug_IDV_TrustCert=1; Debug_Namelookup=1) and the problem was there smiling at me :)

For password reset operation, there are cross certification documents for each resetters. For example, if an administrator (John May/Acme) will reset password of a user (Mary Jane/Acme), there should be a cross-certification document (O=Acme \>\> John May/Acme) for password reset operations. Server will find this document first and validate it with the organization certifier. An example:

```
[0B30:009A-0CC4] NAMELookup::<lookup> Searching view '($Users)' (1 of 1 views).
[0B30:009A-0CC4] NAMELookup::<lookup> Searching name='O=Acme' (1 of 1 names).
[0B30:009A-0CC4] NAMELookup::<lookup> Searching DBIndex=1.
[0B30:009A-0CC4] NAMELookup::<lookup> NumReturned=0, TotalNumReturned=0 match(es) for name='O=Acme'</blockquote>

In this case, it could not find the certifier document (entry not found in index). In my case, though:

<blockquote>[0B30:009A-0CC4] NAMELookup::<lookup> Searching view '($Users)' (1 of 1 views).
[0B30:009A-0CC4] NAMELookup::<lookup> Searching name='O=Acme' (1 of 1 names).
[0B30:009A-0CC4] NAMELookup::<lookup> Searching DBIndex=1.
[0B30:009A-0CC4] NAMELookup::<lookup> NumReturned=2, TotalNumReturned=2 match(es) for name='O=Acme'
```

<br />

<br />

There were two matches in the address book. I just checked with '($Users)' view and that was correct. There were a person document with 'O=acme' line in shortname field!

Probably we made a mistake while dealing with the SSO issue. It can be fatal to place your certifier name into an alias :)

I am a bit flushed... But the problem has been solved...
