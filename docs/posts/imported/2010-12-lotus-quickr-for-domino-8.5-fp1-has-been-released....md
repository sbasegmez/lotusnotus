---
authors:
  - serdar

title: "Lotus Quickr for Domino 8.5 FP1 has been released..."

slug: lotus-quickr-for-domino-8.5-fp1-has-been-released...

categories:
  - News

date: 2010-12-06T14:55:41+02:00

tags:
  - announcement
  - lotus-quickr
---

Lotus Quickr for Domino 8.5 FP1 has been released last friday.
<!-- more -->
You can download the fix pack from [here](http://www-933.ibm.com/support/fixcentral/swg/quickorder?parent=ibm/Lotus&product=ibm/Lotus/Lotus+Domino&release=8.0.2&platform=Windows&function=fixId&fixids=&includeRequisites=1&downloadMethod=ddp&source=fc)...

The summary of current fixes:

|------------|--------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 11/23/2010 | TJOR8AMMCN LO55878 | After adding a member to a room the room manager is taken back to the room's home page instead of staying in the manage room members page.                                                                |
| 11/23/2010 | SHEZ8B3CPE LO56228 | No error message is given if you enter more text in the rich text editor than is allowed, when saved, the text is truncated.                                                                              |
| 11/23/2010 | HHIE8AZ8M4 LO56156 | When uploading images to a blog, you may get an authentication dialog box.                                                                                                                                |
| 11/23/2010 | DTEG8BFC5B         | In certain circumstances the Notes Connector can cause the Quickr Domino server to crash.                                                                                                                 |
| 11/19/2010 | CPRE868R2W         | Under certain configurations using Dual Directory, user lookup may display the wrong DN.                                                                                                                  |
| 11/19/2010 | RALF8B2UWS LO56251 | After creating a large number of folders, you may receive the error "You do not have permission for this action." followed by the error "An item with the same name already exists in the target folder." |
| 11/19/2010 | CSTS8AUQQH LO56098 | Within creating a Link in a Tabbed folder, clicking the link brings you to the edit link page, not the link itself.                                                                                       |
| 11/19/2010 | HMON8AWNSM LO56175 | Using Quickr on iSeries, Connector report the modified times as GMT instead of the GMT offset affiliated with the time zone the server is in.                                                             |
| 11/19/2010 | JYJG8AQ97X         | Users in a local group get prompted for \&nbsp;basic authentication when previewing an attachment.                                                                                                        |
| 11/19/2010 | JYJG8A7BQT LO55517 | Attached images with a large number of pixels do not completely display in preview.                                                                                                                       |
| 11/04/2010 | RALF8A7Q5H LO55546 | Search is returning invalid XML data instead of the search results.                                                                                                                                       |
| 11/04/2010 | CPRE868R2W         | With certain configurations using Domino Dual Directory, a user lookup may return the incorrect DN.                                                                                                       |
| 11/04/2010 | TJOR8AGLA2 LO55776 | The "Log Out" link isn't showing in a place when the Domino server configuration is set to use internet site documents.                                                                                   |
| 11/04/2010 | MMOI88PSLZ LO54334 | For Domino Native Authentication, going directly to a place when the Domino server has Internet Site Documents enabled results in the user being prompted to login.                                       |
| 11/04/2010 | TMAI8AEBQT LO55674 | Large files are not uploaded correctly when QuickPlaceLargeRequestOnFile=1 is set in the notes.ini                                                                                                        |
| 11/04/2010 | HMON8AUQM9 LO56097 | With Connections integration, even though the Team Space is configured to show Members links for Editors and Authors, the Members link does not appear in the left-hand navigation.                       |
| 11/04/2010 | CWIR87BJWY         | Fix for potential crash during LDAP lookup.                                                                                                                                                               |
| 10/26/2010 | CSTS89SS94 LO55240 | More option appears in a blog entry when there is nothing more to display.                                                                                                                                |
| 10/26/2010 | JYJG8A67N9 LO55449 | A document whose title contains Chinese characters can not be edited using the Edit link.                                                                                                                 |
| 10/26/2010 | ESEO89JKU9 LO54970 | New imported file or upload defaults to the top level of the place instead of the folder you are already in.                                                                                              |
| 10/26/2010 | HMON89LP3F LO55042 | When creating a new form, the drop down list containing a list of folders does not appear in a specific order.                                                                                            |
| 10/26/2010 | DWHN89XL95 LO55323 | With h_versiontype=explicit set in the notes.ini, an error is produced when customizing forms.                                                                                                            |
| 10/26/2010 | KCLU89LN94         | Page access changes from private to public after being edited by a place manager or owner.                                                                                                                |
| 10/26/2010 | RALF8AKMCJ LO55823 | Upgrading a Quickr 8.2 place to 8.5 removes display of attachments for comments which get converted to responses after the upgrade. \&nbsp;Attachments are still there, just do not display.              |
| 10/19/2010 | JMAA89PAWC         | ECM Search returns blank page.                                                                                                                                                                            |
| 10/19/2010 | CWIR87BJWY         | LDAP buffer checking.                                                                                                                                                                                     |
| 10/13/2010 | AGOR89LLHZ         | Connections Communities return link is missing from a Place.                                                                                                                                              |
| 10/13/2010 | BBAR89RKL3         | Under certain circumstances searching ECM may crash the Quickr Domino server.                                                                                                                             |
| 10/13/2010 | HMON89PQQM LO55083 | When sending notifications using the 8.2 theme in Quickr 8.5, dashes in host names get removed.                                                                                                           |
| 10/13/2010 | CRAY89XRX2         | Quickr 8.2 places on a Quickr 8.5 server should work without being upgraded.                                                                                                                              |
| 10/13/2010 | TDON84VL8C LO51011 | Qptool report -PolicyExecute fails because of the server contains many places. \*Please note, this fix should reduce the amount of memory qptool uses so should fix all qptool memory related issues.     |
| 10/13/2010 | CWIR87BJWY         | In certain circumstances, the Quickr Domino server may crash when doing LDAP lookups                                                                                                                      |
| 10/13/2010 | RALF89DKR5 LO54880 | What's New Section Not Showing Calendar Events And Task.                                                                                                                                                  |
| 09/28/2010 | MPUL89KLXB LO54981 | Upgrading An 8.2 place with an older theme breaks the Room Members link.                                                                                                                                  |
| 09/28/2010 | NZSG88U5WZ         | Under certain conditions moving a folder via the web client may not retain the description of the folder after the move.                                                                                  |
| 09/28/2010 | SCHA88FDRZ         | A JavaScript error is produced when bringing up a list's properties multiple times.                                                                                                                       |
| 09/28/2010 | CRAY89AN4L         | Under certain conditions, after checking in a Link page and then opening that page produces a blank URL.                                                                                                  |
| 09/28/2010 | XMYG876E77         | For Tasks, the Task start / due date may be incorrect if the client timezone is changed to a timezone that is negative GMT, such as GMT -08:00                                                            |
| 09/28/2010 | JHHO88FDTX         | With the Member Picker, nested groups won't display when creating a page in a restricted folder.                                                                                                          |
| 09/28/2010 | KHON88HMQ3         | All Day Events appear on a calendar the day before it are scheduled for.                                                                                                                                  |
| 09/28/2010 | CRAY88NNKF         | Returning to the Calendar view after creating or editing a calendar entry, the Calendar view will default back to the default Calendar view.                                                              |
| 09/28/2010 | CRAY89GL2R         | Changing a Page access to private and then back to public, the second access change doesn't get applied.                                                                                                  |
