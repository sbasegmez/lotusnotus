---
authors:
  - serdar

title: "New horizons for Lotus Professionals: Information Security (3)"

slug: new-horizons-for-lotus-professionals-information-security-3

categories:
  - Articles

date: 2010-06-27T14:06:41+02:00

tags:
  - security
  - series
  - domino-dev
---

A recent news article claims that the database application keeping land registy records of Istanbul province has been stolen and being marketed to some real estate agencies. It is concerned what may happen if mobs acquire this sensitive information. It reminds us the importance of **information security** .
<!-- more -->
I see that many application developers around are uneducated about generic frameworks. Hence, applications are mostly developed under ad-hoc models shaped with micro-level targets. These ad-hoc models prioritize '**access security** ' instead of '**information security** '. There is a wrong assumption that 'if you let users see the information, you would not prevent it to be stolen'. Access security depends on 'who has access to what'. It mostly relates with process-centric applications. In knowledge-centric projects, it will not be enough. Let me explain this.

Think of our Lotus Notes environment. We have multiple layers of security. ID-based authentication assures we are dealing with the right person as a user. Beside server-level access controls, database ACL decides who will access and be able to do what (create/edit/delete). At the next level, a specific document might be assigned to a specific group of people with reader and author fields. Deep down, we might also prevent someone to see a specific field with field-level-encryption. Take an example of member management application.

We have a customer records database (each customer is a document). We don't want all employees to access to customers. Sales reps, customer reps and accounting guys will be enough (ACL). Sales team should be able to access lead customers. Current customers should be accessed by only their customer reps and accounting department (Reader field). Credit card field should be blocked to anyone but accounting responsible. So we encrypted this field. So far, we have built a fine access security model with our traditional weapons.

Information security is a bit more than access restrictions. For instance, the authorization of an accounting guy to see **credit card information** is based on his function to **withdraw** . However, there is always a risk of an evil-minded employee to exploit this sensitive information. Access restriction should grant the right 'to collect money from customer's account', not 'to see credit number'. For this example, credit card information might be secured (by encryption) and virtual pos application may be used for payments. Most of e-commerce sites are using this methodology to secure credit card information; no one but the bank could access it.

On the other side, we cannot encrypt or hide most of the information we are using. An evil sales rep could get all your lead customers' contact data and go to your competitor. There should be more considerations in your application.

The most common leakage is massive transfer of data. If your application give users opportunity to export all data to Excel, you have a serious problem with it. There are several practical ways to prevent it. For example, in Notes application, you can disable export parameters in the notes.ini file at the startup. You should not display any sensitive information (e-mail address, phone, address, etc.) on view columns because they can easily be copied over. More paranoid precaution might be to divide sensitive information to another document or database which actually complicates your development but a good measure. Using XPages, you can prevent users to access document repository directly which secures them.

Extensive use of monitoring is another action for more security. If an avarage sales rep uses 15-20 customer records in an ordinary day, accessing 500 records should ring some bells! That kind of measures are easily set up in relation databases but I know there are some nsf hooks in the market.

These are application-level considerations. However, there are more...

Analyst reports of some consulting firms might cost thousands of bucks. Documents aggregated at audit companies may contain extremely sensitive information about their customers. A possible leakage costs vital consequences for companies. Applications working on information security audits your documents and control their flows inside and outside your borders. Good example may be this [demo video](http://connect.websense.com/mbh) of Websense product. The software agent scans documents in your system, generates some identification patterns and blocks their transfer between different control points even it is converted to different formats. You cannot print, upload or send the sensitive document in any way.

Removing diskette drives or disabling usb sticks are no longer enough. Information security becomes more and more important subtopic in corporate information systems. Especially telecom companies, financial institutions and auditors have to be adapted in compliance rules which prioritize security issues. Therefore, Lotus professionals should adapt these aproaches in both infrastructure and software development aspects, since Lotus products are vital parts of corporate systems.
