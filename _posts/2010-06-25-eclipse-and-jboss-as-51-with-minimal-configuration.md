---
title: Eclipse and JBoss AS 5.1 with minimal configuration
layout: post
tags: ["java","eclipse","jboss"]
date: 2010-06-25
commentsUrl: http://www.briskbee.com/2010/06/eclipse-and-jboss-as-51-with-minimal.html#comment-form
redirect_from: "/posts/eclipse_and_jboss_as_5_1_with_minimal_configuration/"
---

As I explained yesterday I managed to improve startup times of my JBoss AS 5.1 server dramatically by fiddling a little with the web profile provided by Red Hat. It did however give me some problems when I wanted to start/stop/deploy my web project from Eclipse since this all of a sudden did not work anymore. I decided to fix this and this is what I did.

## Use the JRMP invoker proxy

Either copy the <code>jmx-invoker-service.xml</code> from the default profile into your <code>deploy</code> directory of your profile or apply these changes to your <code>jmx-invoker-service.xml</code> file.

Change

```
<mbean code="org.jboss.invocation.jrmp.server.JRMPProxyFactory" name="jboss.jmx:type=adaptor,name=Invoker,protocol=http, service=proxyFactory">
   <!-- Use the HTTP Invoker from conf/jboss-service.xml -->
   <depends optional-attribute-name="InvokerName">
      jboss:service=invoker,type=http
   </depends>
   ...
</mbean>
```

to

```
<mbean code="org.jboss.invocation.jrmp.server.JRMPProxyFactory" name="jboss.jmx:type=adaptor,name=Invoker,protocol=jrmp, service=proxyFactory">
   <!-- Use the standard JRMPInvoker from conf/jboss-service.xml -->
   <depends optional-attribute-name="InvokerName">
      jboss:service=invoker,type=jrmp
   </depends>
   ...
</mbean>
```

and

```
<mbean code="org.jboss.jmx.connector.invoker.MBeanProxyRemote" name="jboss.jmx:type=adaptor,name=MBeanProxyRemote,protocol=jrmp">
   <depends optional-attribute-name="MBeanServerConnection">
      jboss.jmx:type=adaptor,name=Invoker,protocol=http,service=proxyFactory
   </depends>
</mbean>
```

to

```
<mbean code="org.jboss.jmx.connector.invoker.MBeanProxyRemote" name="jboss.jmx:type=adaptor,name=MBeanProxyRemote,protocol=jrmp">
   <depends optional-attribute-name="MBeanServerConnection">
      jboss.jmx:type=adaptor,name=Invoker,protocol=jrmp,service=proxyFactory
   </depends>
</mbean>
```

## Enable the listening port for the JNP service

Edit the MBean NamingService in the <code>jboss-service.xml</code> file found in the <code>conf</code> directory.

Change

```
<attribute name="Port">-1</attribute>
```

to

```
<attribute name="Port">
   <value-factory bean="ServiceBindingManager" method="getIntBinding">
      <parameter>jboss:service=Naming</parameter>
      <parameter>Port</parameter>
   </value-factory>
</attribute>
```

Voila! You should now be able to start, stop and deploy your web apps in Eclipse.
