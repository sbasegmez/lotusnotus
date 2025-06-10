---
authors:
  - serdar

title: "DOTS Deep Dive 3: Warning for Deadlocks"

slug: dots-deep-dive-3-warning-for-deadlocks

categories:
  - Articles

date: 2013-02-14T16:00:00+02:00

tags:
  - dots
  - java
  - plugins
  - series
  - xpages
---

Last time, I have blogged about the importance of [the importantance of canceling tasklets](2013-02-dots-deep-dive-2-cancel-me-or-i-will-crash-your-server....md "dots-deep-dive-2-cancel-me-or-i-will-crash-your-server....htm")...

In most of the time, canceling a task is a 'choice' you have. You might want to stop the task for a reason. However, a very important problem is falling into deadlocks. If somehow your code falls into a deadlock or stuck situation, that would lock your DOTS container entirely.
<!-- more -->
DOTS uses a basic mechanism for identifying scheduled tasklets that are stuck. Every tasklet starts its life with a predefined timeout value. This is 5 minutes by default. You can use "@HungPossibleAfter( timeInMinutes=n )" annotation to change this value.

Native part of DOTS implementation (ndots task) continuously signals the service thread (thread responsible from running scheduled tasklets) for a new tasklet. As we said before, it is designed to run a single tasklet at a time. But service thread still stores the next tasklet in the queue (that's why scheduling becomes complicated as in the [first post](2013-02-dots-deep-dive-1-art-of-scheduling-tasklets.md "dots-deep-dive-1-art-of-scheduling-tasklets.htm") of this series).

If added tasklet is already in progress, service thread will check if this tasklet has exceeded the timeout value. If so, it warns us...

Here, we have an issue. Because DOTS **do nothing** to cancel the tasklet that has been stuck with a loop. It just throws stack traces and warnings into console to let you know the situation. You have to cancel your tasklet manually. I hope this would be changed in the future.

Let's look around the issue with a sample code:

```java
@RunEvery( every=15, unit=RunUnit.second )
public void run15(IProgressMonitor monitor) throws Exception {
       logMessage("RUN: 15 seconds");
}

@RunEvery( every=1, unit=RunUnit.minute )
@HungPossibleAfter( timeInMinutes=1 )
public void run5(IProgressMonitor monitor) throws Exception {
       logMessage("START: 1 minutes ");

       while(true) {
               Thread.sleep(1000);

               if(monitor.isCanceled()) {
                       logMessage("Cancel request taken...");
                       break;
               }

               if(getSession().getEnvironmentString("waiting_for_star_wars_disney_edition", true).equals("never")) {
                       break;
               }
       }
       
       logMessage("END: 1 minutes");
       
}
```


There are two tasklets here. The first one is simple. The second one is an infinite loop. When we load DOTS, after a point it will be hung.

```
14.02.2013 15:43:09   [DOTS] (stest01) RUN: 15 seconds
14.02.2013 15:43:09   [DOTS] (stest01) START: 1 minutes
14.02.2013 15:45:09   [DOTS] Service Thread Scheduled Runs appears to be stuck on task stest01@run5. It has been more than 2 minute(s) 0 seconds since the last time a task ran. Throwing away new Task instances. Thread Stack trace to follow
14.02.2013 15:45:09   [DOTS] Thread Scheduled Runs appears to be deadlocked:
14.02.2013 15:45:09   [DOTS]    java.lang.Thread.sleep(Native Method)
14.02.2013 15:45:09   [DOTS]    java.lang.Thread.sleep(Thread.java:853)
14.02.2013 15:45:09   [DOTS]    org.openntf.news.playground.tasklets.SchedTest01.run5(SchedTest01.java:32)
14.02.2013 15:45:09   [DOTS]    sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
14.02.2013 15:45:09   [DOTS]    sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:60)
14.02.2013 15:45:09   [DOTS]    sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:37)
14.02.2013 15:45:09   [DOTS]    java.lang.reflect.Method.invoke(Method.java:611)
14.02.2013 15:45:09   [DOTS]    com.ibm.dots.task.ServiceThread$ServiceTask.run(ServiceThread.java:171)
14.02.2013 15:45:09   [DOTS]    com.ibm.dots.task.ServiceThread.run(ServiceThread.java:286)
14.02.2013 15:46:09   [DOTS] Service Thread Scheduled Runs appears to be stuck on task stest01@run5. It has been more than 3 minute(s) 0 seconds since the last time a task ran. Throwing away new Task instances. Thread Stack trace to follow
14.02.2013 15:46:09   [DOTS] Thread Scheduled Runs appears to be deadlocked:
14.02.2013 15:46:09   [DOTS]    java.lang.Thread.sleep(Native Method)
14.02.2013 15:46:09   [DOTS]    java.lang.Thread.sleep(Thread.java:853)
14.02.2013 15:46:09   [DOTS]    org.openntf.news.playground.tasklets.SchedTest01.run5(SchedTest01.java:32)
14.02.2013 15:46:09   [DOTS]    sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
14.02.2013 15:46:09   [DOTS]    sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:60)
14.02.2013 15:46:09   [DOTS]    sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:37)
14.02.2013 15:46:09   [DOTS]    java.lang.reflect.Method.invoke(Method.java:611)
14.02.2013 15:46:09   [DOTS]    com.ibm.dots.task.ServiceThread$ServiceTask.run(ServiceThread.java:171)
14.02.2013 15:46:09   [DOTS]    com.ibm.dots.task.ServiceThread.run(ServiceThread.java:286)
> tell dots cancel
14.02.2013 15:46:35   [DOTS] Canceling all running tasks...
14.02.2013 15:46:35   [DOTS] 1 task(s) cancelled
14.02.2013 15:46:36   [DOTS] (stest01) Cancel request taken...
14.02.2013 15:46:36   [DOTS] (stest01) END: 1 minutes
14.02.2013 15:46:36   [DOTS] (stest01) RUN: 15 seconds
14.02.2013 15:46:39   [DOTS] (stest01) RUN: 15 seconds
```


As you see, the second tasklet run and became deadlocked. If you leave it, it will shout forever but will never ever dispose the tasklet. As soon as we cancel this tasklet, the first one runs twice. "15:46:36" is because it has been queued. "15:46:39" is the normal schedule.

As a result, you have to be very careful about **infinite loops** . Occasional pings to monitoring desk might be a good idea for mission critical tasklets. Always catch the cancel signal. Because if you stop DOTS, it will eventually kill your tasklet but server will be unstable afterwards.

BTW, there are some situations that you don't have a control on catching a cancel signal. For instance, if your tasklet opens a URL connection and does not define a timeout value for the connection, it would wait forever in case remote server does not respond. Always set timeouts :)

Have fun...
