---
authors:
  - serdar

title: "New Horizons for Lotus Developers: Web Services (4)"

slug: new-horizons-for-lotus-developers-web-services-4

categories:
  - Misc

date: 2010-08-23T16:04:00+02:00

tags:
  - domino-dev
  - series
---

Ooops, wrong title :)

"Web Service" is not a new feature for Lotus Domino. But as chatting with Lotus pro's around, I think more and more applications may be developed using web services...
<!-- more -->

#### What is it?

<br />

"Web Service" is a bunch of technologies and protocols to define API's accessed via HTTP and executed on a remote system hosting the requested services. It has been used for over a decade. The W3C defines a "Web service" as "*a software system designed to support interoperable machine-to-machine interaction over a network* ". [More definitions](http://en.wikipedia.org/wiki/Web_service)can be found on Wikipedia... But the basic philosophy behind "Web Service" is to **distribute processing to different applications via 'services' in a 'defined' way** .

At the B.C. times, when developers need some external processing and/or information sharing, they were calling the 'other' developer and trying to agree on ground rules. Consultant were calling this 'methodology' as **Electronic Data Interchange** . There were numbers of problems with EDI such as problems in negotiation and change management.

![Image:New Horizons for Lotus Developers: Web Services (4)](../../images/imported/new-horizons-for-lotus-developers-web-services-4-M2.png)

At the beginning of 2000s, IBM and Microsoft saw an opportunity for standardization and co-developed (with Ariba) the first version of WSDL (Web Services Definition Language). That was the beginning of a new era for software development. You know the rest of the story. IBM took J2EE to 'multiple platform, single language' and MS created .Net platform for 'multiple language on a single platform'.

While the classical architecture is remote procedure call (RPC) methodology, web services have been evolved to Service-oriented architecture in enterprise systems and REST architectures in web 2.0 applications after a decade.

#### Why?

<br />

To repeat, the basic philosophy is distributing processing to various applications. When you design a procurement application, you touch to organization data, approval engine, warehouse records, invoicing system, etc. You may design all functions in a single application or you distribute different parts to responsible systems.

As we are not building ERP applications, let's identify why we should use web services in our domino applications.

Suppose we are developing a simple workflow application for approving expenses. Let's do it in the simplest way. We have a view that contains all employees with their managers. Build another view for categories (a category may be a project or customer). A user creates a document, enlist his/her expenses and submits for approval. After manager's confirmation, it will be on the table of accounting guy where he transfers the information to the accounting database (to transfer money to the employee and/or to add the cost to the project/customer).

Suppose your company is using SAP HR module for employee management and a .Net application for accounting.

The first challange here is the employee listing. Try to explain your HR department manager that they should update a Domino database for every HR operation. Since it's not possible, you should integrate your system into SAP HR module. You have possible techniques here. You may ask SAP team to create a text file periodically, hand you an SQL query, create an RPC for listing, etc. Whichever method you are using, the data will not be available real time to your application.

The second challange is to submit approved expense information to the accounting application. Reading data may be easy, but submitting data will not. Many applications do not operate simple 'insert' operations at this point. They should carry out some checks, insert data to a number of tables and update several more. Therefore, instead of executing a simple insert query, you have to ask developers to build some code. There are options; prepare a text file to be processed, submit information via stored procedures etc. Since the operation will not be real-time, in case of any error, your application will not be aware.

In this case, we have distributed processing to different applications. So the question is still valid. Why do we need Web services?

Let's face another challanges. Either you or remotes system has changed platform. For example, .net application has been moved to an Oracle database or Linux operating system. You have to perform some more meetings with other developers, re-negotiate changes. Now, you are part of their change management team. Since the integration is not real-time, I can assure you that accounting departments will frequently disturb you to run transfer agents out of schedule, especially at payroll dates.

A signed PDF may be required to be stored due to compliance issues. Remember, it is not possible to transfer BLOB data via text files. Text files cannot be validated 'on-the-fly'. However, by using web services, integrating party may validate data using DTDs or schemas before submission.

A more important advantage is topologic issues. If you have a distributed network structures, as there are integration points out of local network, securing file sharing or DBMS access is far more difficult than HTTP ports.

Reusability is another important motivation behind web services. Sometimes, enterprises use hundreds of different systems in different platforms. Most of these cases, developer may not have an option to select the best platform. Your company's .net application may have to exchange data with you and several other data sources. That leads standardization point of view for all developers. You develop a standard web service compatible with your business logic and expect other parties to be adapted.

#### Where to use?

<br />

Let's give you some real-life examples:

* We implemented a "Web Service Provider" for a current customer which imports actionable information coming from PHP web sites into a Notes database. The application processes these information and sends them to related databases for manual and automated actions.
* Ferhat, blogger of "[bestcoder.net](http://www.bestcoder.net/)" uses web services to integrate Blackberry applications into Domino databases.
* One customer implemented a self service application that provides users to reset their own passwords using ID Vault. "Web Service Consumer" uses a .net application to submit the random password via SMS.
* Another customer implemented a "Web Service Consumer" to get organization hierarchy from SAP HR module into Workflow Organization database.
* Another customer integrated a Domino-based CRM application to hotel management software via web services. Hotel management software validates customer's discount level using web services.

<br />

#### How to start?

<br />

I couldn't find the exact source, but it's cool: "***Web services are like high school sex. Everyone is talking about doing it, but hardly anyone is, and those that are probably aren't doing it well.*** ". As in object orientation, defining a web service is not easy. You just begin with basics. In time, you fail, redesign, fail, redesign and build your own best practices.

In Domino, you can easily provide and consume web services. Following documents are good points to start for 'dummies'...

* [Lotusphere 2004 : JMP103 - Introduction To Web Services](http://www-10.lotus.com/ldd/sandbox.nsf/ecc552f1ab6e46e4852568a90055c4cd/5171a849ce49f47e85256e2f0055ceca?OpenDocument)
* [Lotusphere 2007: AD509 - Using Web Services Features in IBM Lotus Notes and Domino](http://www-10.lotus.com/ldd/sandbox.nsf/ecc552f1ab6e46e4852568a90055c4cd/7cc00980257e905485257291004c24bb?OpenDocument)
* [Practical Web Services in IBM Lotus Domino 7: What are Web services and why are they important?](http://www.ibm.com/developerworks/lotus/library/web-services1/index.html)
* [Practical Web services in IBM Lotus Domino 7: Writing and testing simple Web services](http://www.ibm.com/developerworks/lotus/library/web-services2/index.html)
* [Practical Web services in IBM Lotus Domino 7: Writing complex Web services](http://www.ibm.com/developerworks/lotus/library/web-services3/index.html)
* [Web Services - What's new? (in 8.5)](http://www-10.lotus.com/ldd/ddwiki.nsf/dx/web-services-whats-new.htm)

<br />

*(There are small changes from Lotus Domino 7 to 8.x. There were no native consumer in version 7. So, some examples guide using Java classes to consume web services.)*

We are developing a customer application to provide some web services for multiple integration points. I want to share its details upon its completion next weeks, hopefully...
