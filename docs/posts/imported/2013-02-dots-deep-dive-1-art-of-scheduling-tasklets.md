---
authors:
  - serdar

title: "DOTS Deep Dive 1: Art of Scheduling Tasklets"

slug: dots-deep-dive-1-art-of-scheduling-tasklets

categories:
  - Articles

date: 2013-02-11T12:05:00+02:00

tags:
  - dots
  - java
  - plugins
  - series
  - xpages
---

After a [successful IBM Connect session](2013-02-a-rookie-speaker-was-here....md "a-rookie-speaker-was-here....htm"), I started a series of posts, based on feedbacks I received from other developers.

There was a little thing I didn't test before the session and this issue has been asked a couple of times: Possible conflicts between scheduled tasklets.
<!-- more -->
Unfortunately, current implementation within DOTS is based on single threaded approach for tasklets. There are three different threads responsible in DOTS tasklet container for scheduled, manual and triggerred tasklets. It means, there would be only one thread alive to run scheduled tasklets, therefore, only one scheduled tasklet can run at a time.

Let's dive into possible scenerios here.

Suppose we have a scheduled tasklet that will run every 5 seconds and wait for 7 seconds:

```
11.02.2013 11:10:19   [DOTS] (stest01) START: 5 Secs
11.02.2013 11:10:26   [DOTS] (stest01) END: 5 secs
11.02.2013 11:10:28   [DOTS] (stest01) START: 5 Secs
11.02.2013 11:10:35   [DOTS] (stest01) END: 5 secs
11.02.2013 11:10:38   [DOTS] (stest01) START: 5 Secs
11.02.2013 11:10:45   [DOTS] (stest01) END: 5 secs
```


As you see, since it runs longer than its schedule, it will pass 5 secs schedule and runs every 10 seconds.

Suppose we add a second tasklet here, to run every 2 seconds but wait for 1 secs:

```
11.02.2013 11:11:15   [DOTS] (stest01) START: 2 Secs
11.02.2013 11:11:16   [DOTS] (stest01) END: 2 secs
11.02.2013 11:11:16   [DOTS] (stest01) START: 5 Secs
11.02.2013 11:11:23   [DOTS] (stest01) END: 5 secs
11.02.2013 11:11:23   [DOTS] (stest01) START: 2 Secs
11.02.2013 11:11:24   [DOTS] (stest01) END: 2 secs
11.02.2013 11:11:24   [DOTS] (stest01) START: 5 Secs
11.02.2013 11:11:31   [DOTS] (stest01) END: 5 secs
11.02.2013 11:11:31   [DOTS] (stest01) START: 2 Secs
11.02.2013 11:11:32   [DOTS] (stest01) END: 2 secs
11.02.2013 11:11:33   [DOTS] (stest01) START: 5 Secs
11.02.2013 11:11:40   [DOTS] (stest01) END: 5 secs
11.02.2013 11:11:40   [DOTS] (stest01) START: 2 Secs
11.02.2013 11:11:41   [DOTS] (stest01) END: 2 secs
11.02.2013 11:11:42   [DOTS] (stest01) START: 2 Secs
11.02.2013 11:11:43   [DOTS] (stest01) END: 2 secs
11.02.2013 11:11:43   [DOTS] (stest01) START: 5 Secs
```


This is a complicated scheduling right? They will never run at the sametime. If a tasklet should run in 15th, 20th, 25th and 30th seconds but there is another tasklet running at 20th and 30th seconds, it will just skip.

This is a big problem if you are going to use lots of long-running scheduled tasklets. A tasklet running longer than its schedule would not be a huge problem but multiple tasklets disrupting each other?

OK, there is a very nice solution for that:

Now we will have one tasklet running every 5 secs and waiting for 30 secs (very long job) and another one running every 3 secs waiting 2 secs. Here they are running friendly without intervening each other:

```
11.02.2013 11:49:06   [DOTS2] (stest01) START: 5 Secs
11.02.2013 11:49:07   [DOTS] (stest01) END: 3 secs
11.02.2013 11:49:08   [DOTS] (stest01) START: 3 Secs
11.02.2013 11:49:10   [DOTS] (stest01) END: 3 secs
11.02.2013 11:49:11   [DOTS] (stest01) START: 3 Secs
11.02.2013 11:49:13   [DOTS] (stest01) END: 3 secs
11.02.2013 11:49:14   [DOTS] (stest01) START: 3 Secs
11.02.2013 11:49:16   [DOTS] (stest01) END: 3 secs
11.02.2013 11:49:17   [DOTS] (stest01) START: 3 Secs
11.02.2013 11:49:19   [DOTS] (stest01) END: 3 secs
11.02.2013 11:49:20   [DOTS] (stest01) START: 3 Secs
11.02.2013 11:49:22   [DOTS] (stest01) END: 3 secs
11.02.2013 11:49:23   [DOTS] (stest01) START: 3 Secs
11.02.2013 11:49:25   [DOTS] (stest01) END: 3 secs
11.02.2013 11:49:26   [DOTS] (stest01) START: 3 Secs
11.02.2013 11:49:28   [DOTS] (stest01) END: 3 secs
11.02.2013 11:49:29   [DOTS] (stest01) START: 3 Secs
11.02.2013 11:49:31   [DOTS] (stest01) END: 3 secs
11.02.2013 11:49:32   [DOTS] (stest01) START: 3 Secs
11.02.2013 11:49:34   [DOTS] (stest01) END: 3 secs
11.02.2013 11:49:35   [DOTS] (stest01) START: 3 Secs
11.02.2013 11:49:36   [DOTS2] (stest01) END: 5 secs
11.02.2013 11:49:37   [DOTS] (stest01) END: 3 secs
11.02.2013 11:49:38   [DOTS] (stest01) START: 3 Secs
11.02.2013 11:49:40   [DOTS] (stest01) END: 3 secs
11.02.2013 11:49:41   [DOTS2] (stest01) START: 5 Secs
```


Notice something different? They are running in different DOTS tasklet containers. 3-sec tasklet is running inside DOTS profile and the other one is running in DOTS2.

Multiple profiles are great if you run critical and complicated DOTS tasklets because it has its own **Tasklet Container** ...

However, multiple profile helps you if you have seperate projects. What if you need multiple long-running tasklets within the same project?

For this, you have to enter into a red zone where every Domino-Java developers will eventually experience... In this jungle, you have to create your own thread and manage that. I will provide samples in the future...
