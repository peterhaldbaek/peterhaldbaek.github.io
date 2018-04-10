---
title: CPU spikes on JBoss AS 5.1
layout: post
tags: ["java","jboss"]
date: 2010-06-29
commentsUrl: http://www.briskbee.com/2010/06/cpu-spikes-on-jboss-as-51.html#comment-form
redirect_from: "/posts/cpu_spikes_on_jboss_as_5_1/"
---

While deploying a pretty simple web application on JBoss AS 5.1 (using the default profile) I noticed some weird cyclic CPU spikes even without ANY load on the server. It would start right after the server was started and the spikes would use ~40% of the CPU. Not exactly satisfying. I did the usual trip around the good, old web and quickly found out that the administration console in JBoss AS 5.x would create these hiccups. I deleted the administration console and saw CPU utilization drop to ~10%. That is what I call improvement.

While searching the web I found [this little piece](http://community.jboss.org/wiki/JBoss5xTuningSlimming) on how to optimize JBoss AS 5.x. And yes, it does include removal of the administration console as well.
