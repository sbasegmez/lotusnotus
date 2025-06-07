---
authors:
  - serdar

title: "ID Vault Upload/Download Problems"

slug: id-vault-uploaddownload-problems

categories:
  - Tips & Tricks

date: 2010-03-23T08:20:34Z

tags:
  - domino-admin
  - troubleshooting
---

ID Vault is one of the most important features introduced in release 8.5. It provides remarkable opportunities to solve some annoying problems of system administrators.

Administrators who implement ID Vault feature, mostly deals with ID Download/Upload issues. Therefore I have decided to create this post to share my experiences about these issues. Since ID Vault is a very new feature, there are lots of problems specific to notes and domino version. As you read the rest of the post, you will notice that getting the latest version of the client and server software (8.5.1 FP1) is a very good idea.
<!-- more -->
I don't want to get in detail with who vault mechanism works. To get more information about it, you may read [Domino Security Wikis](http://www.lotus.com/ldd/dominowiki.nsf/xpViewCategories.xsp?lookupName=Domino%20security) pages in developerWorks.

ID Vault, basically creates a secure central database structure to store user id files. Before, some administration operations such as password change, key-rollover or recertification are needed user interaction. With ID vault, these procedures can be completed without any need of user id files' participation. In addition, some of these operations can be delegated into multiple levels of hierarchy. For example reset password administrators could be assigned based on organizational units or self service applications can be developed for users to reset their own password.

After ID Vault is configured and users are assigned to vault via policies; users who login their home servers with Notes client with version 8.5 or higher sends their ID files into the vault database. ID files of new users registered with these policies are also sent to the vault database. We call this 'sending' operation as 'ID Upload'.

Similarly, some events like password reset operation, id file corruption/deletion or multiple login of users who have changed their password initiates a syncronization between vault database and client software. We call this receiving operation as 'ID Download'.

All ID Vault operations are logged in '**Security Events** ' section of server's and client's **log.nsf** database. A little trick about logging issues; sometimes "*Error: Entry not found in index* " error may be misleading for admins just before "*ID successfully synchronized with vault*" message for the same user. Client checks its id file whether it exists on the vault or not and receives this error. So it's normal if it successfully synchronizes after that.

Before FAQ section, I want to remind a couple of details. Firstly, if you see different errors or find another solution about these issues, just post me via comments or e-mail. Second; I will update this post as I find new issues or you send me new incidents.

#### **1. ID Upload Problems**

**1.1.User is assigned to ID Vault, his/her policy has been updated. He/she logged in from a client with R8.5.x but ID file has not been uploaded yet.**

I dealt with this problem during my first testings. After I noticed the documentation it was clear. To load balance the Vault server and database, upload process is being carried out in a random manner. For example in clusters random server is selected and retry times are also calculated randomly. In case of upload failures, you cannot be sure when to be tried again.

Especially on testing phase, with more than one id files used on single client installations, we see similar cases. I usually clear entries with 'IDVault\*' from 'notes.ini' file and restart client to solve.

Check two points in upload issues. One is 'Home Server' field must be correct in the location document. Second is that you should check log file (security events section of log.nsf database on local and/or server) to get some clue about what's going on.

One technote of IBM Support suggests to check personal address book (names.nsf) template version (must be 8.5.x) and policy setting names. '**KeyFileName**' parameter in 'notes.ini' file should be same as the id file used. In one case, the issue has been fixed after id file has been renamed to 'user.id' according to the same technote.

**1.2.'ID File Cannot be Uploaded' error during/after new user registration.**

It is the most common problem I have seen.

The registration process works like that: Administration client creates a temporary ID file into the temp folder. Then it temporarily switches to new ID file and logs in the registration server. The ID file is finally uploaded to the vault server like in normal procedure.

Person document may be created and/or mail database or request to create mail database may be generated in many cases. However, since ID file is missing, registration is not completed. In order to retry, you should delete these changes.

One technote suggests antivirus software may interfere the files created in temporary folders. Looking into antivirus logs may be a good beginning.

In Lotus Domino 8.5, if "Compare public key \> Enforce key checking for all Lotus Notes users and Lotus Domino servers" configuration is being used on the server document of registration server, id file cannot be uploaded since the server limits access of the new id file without person document. Release 8.5.1 fixed this issue. A workaround will be changing to "Compare public key \> Enforce key checking for Notes users and Domino servers listed in trusted directories only" option.

Again in Lotus Domino 8.5, if you are using '**Certification Authority**', id file cannot be uploaded during registration. "Error: Your certificate has not yet been signed by the Certificate Authority. Try again later." message is logged in this case. As you know, CA certification should be validated and approved with administration process. Normally it is an asynchronous process. According to IBM Support, you may ignore this error. The first use of ID file will complete the syncronization. Likewise, it has been fixed in release 8.5.1.

We have experienced a problem in Lotus Notes 8.5 version which is originated from the location settings of administrator user. If 'Mail File Location' is not selected as 'on server' in the location document, the temporary login could not be completed and upload would be failed. Lotus Notes 8.5.1 fixed the issue.

In another customer case, a user with exact the same name had been created and deleted before. In this case ID upload failed with the error "Note Item not found" on log file. We see that older user record is in '**Inactive**' status on the vault database. When we delete that record, the issue has been fixed.

#### **2. ID Download Problems**

**2.1.During client configuration wizard on the fresh install, we get the error "Your password has expired. You cannot access this server until you change it".**

Lotus Notes 8.5.1 release has this bug.To fix the issue, install FP1 for the client, clear the notes.ini file (except the first 6 lines) and restart the wizard.

**2.2.During client configuration wizard on the fresh install, client hangs and does not proceed to the next step.**

I saw this issue during my own tests. Again in Lotus Domino 8.5, when id file is not placed anywhere but vault and ID download rights are limited, you don't see any error message but the wizard does not proceed. The issue has been fixed in release 8.5.1. Workaround is to set ID download count to 1 for that user and try again.

**2.3.User password has been reset but ID cannot be downloaded for some reason.**

In one customer case, user has entered wrong password several times. ID Vault limits download rights for 24 hours after a series of password failures. To reset the password again will fix the problem.

I think it is a good start for now. I will append new issues in time, hopefully.
