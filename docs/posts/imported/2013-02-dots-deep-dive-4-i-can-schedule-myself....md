---
authors:
  - serdar

title: "DOTS Deep Dive 4: I can schedule myself..."

slug: dots-deep-dive-4-i-can-schedule-myself...

categories:
  - Articles

date: 2013-02-21T14:05:00+02:00

tags:
  - dots
  - java
  - plugins
  - series
  - xpages
---

Finally, we will be able to enable FeedMonster for CollaborationToday project.

While doing final touches, I have been challenged by a question: "Can we schedule DOTS tasklets programmatically?"
<!-- more -->
Actually, this is in the wish list for the next version of DOTS. But we can do some trick here. I didn't test this on Domino 9 but it should work.

Here is the code:

```
package org.openntf.news.playground.tasklets;

import org.eclipse.core.runtime.CoreException;
import org.eclipse.core.runtime.IProgressMonitor;

import com.ibm.dots.annotation.RunOnStart;
import com.ibm.dots.task.AbstractServerTaskExt;
import com.ibm.dots.task.RunWhen;
import com.ibm.dots.task.RunWhen.RunUnit;
import com.ibm.dots.task.ServerTaskManager;

public class CustomSchedule extends AbstractServerTaskExt {

       @Override
       public void dispose() {

       }
       
       @RunOnStart
       public void scheduleMe(IProgressMonitor monitor) {
               ServerTaskManager stm=ServerTaskManager.getInstance();
               
               // Every 10 seconds
               RunWhen sched=new RunWhen(RunUnit.second, 10, 0, 0);
               
               try {
                       // custom01 is this tasklet's id as defined in plugin.xml
                       stm.scheduleTasklet("custom01", sched);
               } catch (CoreException e) {
                       e.printStackTrace();
               }
               
               logMessage("I have scheduled myself for "+sched);
               
       }
       
       @Override
       protected void doRun(RunWhen runWhen, IProgressMonitor monitor) {
               logMessage("CustomSchedule: "+runWhen.toString());
       }
       
}
```

<br />

<br />

So it's simple. There is a ServerTaskManager singleton object which manages all tasklets. We are using ".schedule(...)" method (luckily it's public).

When we load DOTS:

```
> load dots
Listening for transport dt_socket at address: 8001
21.02.2013 13:55:30   Domino OSGi Tasklet Container started ( profile DOTS )
21.02.2013 13:55:31   [DOTS] (custom01) I have scheduled myself for Runs every 10 - second
21.02.2013 13:55:33   [DOTS] (custom01) CustomSchedule: Runs every 10 - second
21.02.2013 13:55:43   [DOTS] (custom01) CustomSchedule: Runs every 10 - second
```

<br />

<br />

You can use some kind of parameterization to customize your tasklet scheduling. For instance, I am planning to use this functionality for FeedMonster such that If there are only 5 feeds to be read, it's useless to receive new stories every 5 minutes. However, there is no way to 'unschedule' a tasklet. We'll find another trick for this.

ServerTaskManager is a great object BTW. For instance;

```
stm.runTasklet("someTasklet", "argument1", "argument2");
```

<br />

<br />

I didn't find it safe though. Since it doesn't submit a CommandInterpreter object, there might be a problem in error trapping. Noted to the list of feedbacks :)
