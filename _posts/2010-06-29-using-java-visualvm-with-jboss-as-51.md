---
title: Using Java VisualVM with JBoss AS 5.1
layout: post
tags: ["java","jboss"]
date: 2010-06-29
commentsUrl: http://www.briskbee.com/2010/06/using-java-visualvm-with-jboss-as-51.html#comment-form
redirect_from: "/posts/using_java_visualvm_with_jboss_as_5_1/"
---

[Java VisualVM](https://visualvm.dev.java.net/) is a nice little tool for monitoring any Java application. I used it for monitoring a JBoss AS 5.1 server and had to add some system properties to the run.sh/run.bat file before it would let me connect. The system properties are

```
# Enabling JMX for remote monitoring using port 1234
JAVA_OPTS="$JAVA_OPTS -Dcom.sun.management.jmxremote.port=1234"
JAVA_OPTS="$JAVA_OPTS -Dcom.sun.management.jmxremote.authenticate=false"
JAVA_OPTS="$JAVA_OPTS -Dcom.sun.management.jmxremote.ssl=false"
```

Start the server, start VisualVM and connect to the server either by clicking on the JBoss server (by PID) if VisualVM and the server is running on the same machine or by specifying the server as a remote host (by JMX).
