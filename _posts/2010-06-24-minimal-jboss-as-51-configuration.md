---
title: Minimal JBoss AS 5.1 configuration
layout: post
tags: ["java","jboss"]
date: 2010-06-24
commentsUrl: http://www.briskbee.com/2010/06/minimal-jboss-as-51-configuration.html#comment-form
redirect_from: "/posts/minimal_jboss_as_5_1_configuration/"
---

When I deploy applications on the JBoss AS platform I normally just use the default profile provided by Red Hat. JBoss AS 5.1 is however horrendously slow when starting up so I decided to look into the other (lesser) profiles to see if this would speed things up a bit. I am running a standard web application (packaged as a .war file) based on JSF 1.2 (Sun RI) and Hibernate which accesses an Oracle database.

JBoss AS 5.1 comes with 5 profiles; all, default, minimal, standard and web. I decided to use the web profile since this one (supposedly) supports jdbc and web applications out of the box. As you might guess this was not my experience at first hand since the profile would not recognize my datasource. It came up short with this error message:

```
16:03:47,718 ERROR [ProfileServiceBootstrap] Failed to load profile: Summary of incomplete deployments (SEE PREVIOUS ERRORS FOR DETAILS):

DEPLOYMENTS MISSING DEPENDENCIES:
  Deployment "jboss.jca:name=jdbc/mydatasource,service=DataSourceBinding" is missing the following dependencies:
    Dependency "jboss:service=invoker,type=jrmp" (should be in state "Create", but is actually in state "** NOT FOUND Depends on 'jboss:service=invoker,type=jrmp' **")

DEPLOYMENTS IN ERROR:
  Deployment "jboss:service=invoker,type=jrmp" is in error due to the following reason(s): ** NOT FOUND Depends on 'jboss:service=invoker,type=jrmp' **
```

The solution to this was to copy the <code>legacy-invokers-service.xml</code> file from the default profile directory into the web profile directory and everything deployed nicely and worked as expected. The end result? Before startup would take 54 seconds, now it is down to 12 seconds. And this could probably be optimized more by removing additional unused elements (like support for Hypersonic databases).
