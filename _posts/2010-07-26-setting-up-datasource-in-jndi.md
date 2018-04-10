---
title: Setting up a datasource in JNDI
layout: post
tags: ["java","test","jndi"]
date: 2010-07-26
commentsUrl: http://www.briskbee.com/2010/07/setting-up-datasource-outside-of.html#comment-form
redirect_from: "/posts/setting_up_a_datasource_in_jndi/"
---

Writing code quite often involves writing a lot of unit tests. If your tests cover isolated business logic (standard unit tests) writing tests is usually pretty easy. No big dependencies and very isolated testing. If you are writing tests involving database connections things can get a little more complex. The datasource in an application context is usually stored in JNDI so you need to initialize this yourself when writing tests. I use simple-jndi for this and this is how I do it.

First you need to get the jars from [simple-jndi](http://www.osjava.org/simple-jndi/). If you use Maven you can also just add this to your pom.xml.

```
<dependency>
    <groupId>simple-jndi</groupId>
    <artifactId>simple-jndi</artifactId>
    <version>0.11.4.1</version>
    <scope>test</scope>
</dependency>
```

Initialization of the datasource is done by creating the context and adding the datasource to the context. The context can be defined in the file system (by using the <code>SimpleContextFactory</code>) or you can just store it in memory (by using the <code>MemoryContextFactory</code>). In this example I initialize a datasource for the [HSQLDB](http://hsqldb.org/) database, store it in memory and make the context available for all occurrences of the initial contexts by setting the system property <code>org.osjava.sj.jndi.shared</code> to true.

```
import javax.naming.Context;
import javax.naming.InitialContext;
import org.hsqldb.jdbc.JDBCDataSource;

...

// Create initial context
System.setProperty(Context.INITIAL_CONTEXT_FACTORY, "org.osjava.sj.memory.MemoryContextFactory");
System.setProperty("org.osjava.sj.jndi.shared", "true");
InitialContext ic = new InitialContext();

ic.createSubcontext("java:/comp/env/jdbc");

// Construct DataSource
JDBCDataSource ds = new JDBCDataSource();
ds.setDatabase("jdbc:hsqldb:hsql://localhost/xdb");
ds.setUser("SA");
ds.setPassword("");

// Put datasource in JNDI context
ic.bind("java:/comp/env/jdbc/myDS", ds);
```

This creates a HSQLDB datasource in the context <code>java:/comp/env/jdbc/myDS</code>. The datasource can be retrieved from the code by looking it up in the context like this.

```
InitialContext ic = new InitialContext();
ds = (DataSource)ic.lookup("java:/comp/env/jdbc/myDS");
```

It is as easy as that. Now you can access your database via JNDI.
