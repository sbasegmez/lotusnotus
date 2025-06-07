---
authors:
  - serdar

title: "Launching Lotus Symphony 3.0 inside Lotus Notes Client?"

slug: launching-lotus-symphony-3.0-inside-lotus-notes-client

categories:
  - Tips & Tricks

date: 2010-12-07T11:11:45+02:00

tags:
  - notes-client
---

Using Lotus Symphony 3.0?

When you install Lotus Symphony 3.0 on your computer, there will be an odd situation. There is Lotus Symphony 3.0 installed, but 1.3 inside your Notes client.

If you want to upgrade embedded Symphony 1.3 and use version 3.0 inside your Notes client, you have to use a seperate installation for Symphony. This is called **IBM Lotus Symphony 3.0 Add on Pack** .
<!-- more -->
Part numbers on passport advantage site are:

* IBM Lotus Symphony 3.0 Add on Pack for Windows Multilingual(CZT5PML)
* IBM Lotus Symphony 3.0 Add on Pack for Linux (RPM Install) Multilingual(CZT5RML)
* IBM Lotus Symphony 3.0 Add on Pack for Linux (Debian Install) Multilingual(CZT5TML)
* IBM Lotus Symphony 3.0 Add on Pack for Mac Multilingual(CZT5VML)

<br />

Another strange case is the sizes of installation files:

1.IBM_Lotus_Symphony_w32.exe (Standalone installation) : 258 MB.
2.IBM Lotus Symphony 3.0 Installer and Editor Component for Notes 8.5.x for Windows Multilingual (CZT5NML): 530 MB.
3.IBM_Lotus_Symphony_w32_AddOn.zip : 377 MB.

Logically, the first file should be bigger than others because it has to contain a seperate Eclipse client code underneath. IBM works in mysterous ways :)
