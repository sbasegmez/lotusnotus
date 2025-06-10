---
authors:
  - serdar

title: "DOTS Deep Dive 2: Cancel me or I will crash your server..."

slug: dots-deep-dive-2-cancel-me-or-i-will-crash-your-server...

categories:
  - Articles

date: 2013-02-13T12:11:54+02:00

tags:
  - dots
  - java
  - plugins
  - series
  - xpages
---

I just wanted to emphasize an important functionality within DOTS...

One of our slides in the [recent DOTS session](https://speakerdeck.com/sbasegmez/bp207-meet-the-java-application-server-you-already-own-ibm-domino) in IBM Connect 2013, we have talked about the "***monitor*** " argument in tasklets. It has two important uses.
<!-- more -->
First of all, you might let DOTS container know about your progress. Second, it allows you to cancel your task in a less-disruptive manner.

Let's dive into code here.

Our tasklet is running every five seconds and wait 30 seconds each run:

```java
@RunEvery( every=5, unit=RunUnit.second )
public void run5(IProgressMonitor monitor) throws Exception {
      logMessage("START: 5 Secs");
     
      monitor.beginTask("Sleeping well", 15);

      for(int i=0; i<15; i++) {
              monitor.worked(1);
              Thread.sleep(2000);
      }
     
      logMessage("END: 5 secs");

}
```


We have splitted 30 seconds wait into 15 parts of sleeping to make more sense. Using "*beginTask* " method at start, we have declared that "Our tasklet will do 15 units of job". Each unit, we inform progress monitor for "1 unit completed" message with "*worked* " method. We can check the status from the console:

```
11.02.2013 12:22:03   [DOTS] (stest01) START: 5 Secs
> tell dots taskstatus
11.02.2013 12:22:06   [DOTS] Display the list of OSGi tasklets currently in progress
11.02.2013 12:22:06   [DOTS] Scheduled Runs
11.02.2013 12:22:06   [DOTS] --- stest01 [Sleeping well] (13,333%)
11.02.2013 12:22:06   [DOTS] Triggered Runs
11.02.2013 12:22:06   [DOTS] --- No task running
11.02.2013 12:22:06   [DOTS] Manual Runs
11.02.2013 12:22:06   [DOTS] --- No task running
```


What if we cancel this task? Lets try...

```
11.02.2013 12:28:58   [DOTS] (stest01) START: 5 Secs
> tell dots q
11.02.2013 12:29:02   [DOTS] Canceling task stest01
11.02.2013 12:29:29   [DOTS] (stest01) END: 5 secs
11.02.2013 12:29:29   Domino OSGi Tasklet Container terminated ( profile DOTS )
```


When we stop DOTS task, it will try to cancel our task. However it's not a cancellation. Our tasklet finished its run (30 seconds) because it doesn't have an idea we have sent a cancel signal...

Now add a simple check inside our loop...

```java
@RunEvery( every=5, unit=RunUnit.second )
public void run5(IProgressMonitor monitor) throws Exception {
      logMessage("START: 5 Secs");
     
      monitor.beginTask("Sleeping well", 15);
     
      for(int i=0; i<15; i++) {
              monitor.worked(1);
              Thread.sleep(2000);
             
              if(monitor.isCanceled()) {
                      logMessage("Cancel request taken...");
                      break;
              }
             
      }
     
      logMessage("END: 5 secs");

}
```


When we cancel the tasklet, it will get the signal and break the execution...

```
11.02.2013 12:34:29   [DOTS] (stest01) START: 5 Secs
> tell dots q
11.02.2013 12:34:32   [DOTS] Canceling task stest01
11.02.2013 12:34:33   [DOTS] (stest01) Cancel request taken...
11.02.2013 12:34:33   [DOTS] (stest01) END: 5 secs
11.02.2013 12:34:34   Domino OSGi Tasklet Container terminated ( profile DOTS )
```


Receiving the cancel signal is extremely important. Suppose you are developing a very very long-running tasklet (it might be a manual tasklet as well). You should be able to break the execution at any point without waiting its normal run. At some point of cancellation, DOTS will decide it's crashed and fill your console with strange error messages...

One last point: Cancellation can be achived by "*tell dots cancel (tasknumber)* " command. I couldn't find out what the tasknumber is but if you don't provide a parameter, it will cancel all running tasks at once.
