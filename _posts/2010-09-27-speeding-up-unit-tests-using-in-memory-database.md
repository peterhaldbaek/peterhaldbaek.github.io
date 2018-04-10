---
title: Speeding up unit tests using in-memory database
layout: post
tags: ["java","test"]
date: 2010-09-27
commentsUrl: http://www.briskbee.com/2010/09/speeding-up-unit-tests-using-in-memory.html#comment-form
redirect_from: "/posts/speeding_up_unit_tests_using_in-memory_database/"
---

I am a big fan of test-driven principles but have been on projects where running the full unit test suite would last 2-3 hours simply because writing to and reading from the database would slow everything down. This normally lead to optimizations such as only creating test data once having the unfortunate side effect of making unit tests dependent on each other (one unit test may change some data used in another unit test). It can also lead to random behavior making the test pass one time and then not the other simply because unit tests are not necessarily run in the same sequence every time.

Using an in-memory database can improve time spent on running unit tests significantly. In this article I will discuss the pros and cons of this approach and show how to do this.

Before you start using in-memory databases for unit testing some basic questions should be asked.

1. Does your database support in-memory use at all?
If it does there is no excuse for not using the in-memory database instead of the physical one. Databases I know of who currently support in-memory versions are [SQLite](http://www.sqlite.org/inmemorydb.html), [HSQLDB](http://hsqldb.org/) and [Oracle TimesTen](http://www.oracle.com/technetwork/database/timesten/overview/index.html).
2. Do you use non ANSI SQL?
3. Do you use triggers?
4. Do you use some form of procedural SQL (PL/SQL and the likes)?

If your database does not support in-memory usage and you can answer yes to one (or more) of the questions above it might turn out you cannot use in-memory databases but there are ways to solve this. Lets go into detail.

## Do you use non ANSI SQL?

If your SQL is non ANSI SQL you will have a harder time switching between database vendors since the code now depends on vendor-specific SQL. One such example could be Oracles old definition of joins. In Oracle 8i and older versions outer left joins where defined like this

```
SELECT * FROM emp, dept WHERE emp.deptno = dept.deptno(+)
```

Whereas the newer version (9i and onward) also supports the ANSI SQL definition

```
SELECT * FROM emp LEFT OUTER JOIN dept on emp.deptno = dept.deptno
```

Please be aware that the old definition of left outer (and other) joins are still supported by Oracle meaning that the non ANSI SQL definition of joins still exist in a lot of places (I used to use them myself until I realized this).

If you use Hibernate this is (probably) not a problem since you can just change the database dialect of Hibernate when running your unit tests. But this requires that you do not make any direct use of (non ANSI) SQL and my experience is that this is seldom the case. There is always some special problem which is to hard to express via Hibernate and then you end up writing some SQL instead.

## Do you use triggers?

If you decide to use the in-memory database of HSQLDB (I did) and you have triggers in your (production) database you need to have something similar in your in-memory database. HSQLDB [supports this](http://hsqldb.org/doc/2.0/guide/triggers-chapt.html) in its own peculiar way but it is actually quite neat in a testing environment because the trigger itself is a little piece of Java code that is executed. So you need to implement the triggers in the test database as well.

## Do you use some form of procedural SQL (PL/SQL and the likes)?

If your application makes heavy use of procedural SQL in-memory databases are probably not the best fit unless your production database supports this out of the box. Because you will probably have to rewrite all of your procedures to do so. If it is only a matter of converting one or two procedures an in-memory database like HSQLDB supports this like triggers above. It is just a matter of writing your procedure as some Java code and register it as a procedure in HSQLDB.

## Using an in-memory database

I was working on a project where unit testing was abandoned since running the tests were simply not possible. Not because it took a lot of time but simply because the tests would fail when they were being run together. Database connections were not released quickly enough giving all kinds of problems. So instead of trying to fix a problem I had been trying to solve for years I ended up redefining the whole testing framework making use of an in-memory database. We were using Oracle 10g and the database itself was pretty simple so it made a lot of sense to make the switch. Using HSQLDB in-memory is really easy. I implemented a base test class all unit tests extend from. This base test class is responsible for creating the proper JNDI context containing the HSQLDB data source.

```
import javax.naming.InitialContext;
import org.hsqldb.jdbc.JDBCDataSource;

...

// Construct DataSource
JDBCDataSource ds = new JDBCDataSource();
ds.setDatabase("jdbc:hsqldb:mem:myDB");
ds.setUser("SA");
ds.setPassword("");

// Put datasource in JNDI context
InitialContext ic = new InitialContext();
ic.bind("java:/comp/env/jdbc/myDS", ds);
```

Before each test the database is created with schema, tables and test data. Since everything resides in memory initialization is exceptionally fast compared to a physical database. I never made any direct measurements to compare the two approaches but I know I never wait for a test to finish. It just does in an instant. That is such a relief!

The Hibernate configuration for the test classes must also be changed to use the HSQLDB dialect. Edit this in your hibernate.cfg.xml file:

```
<property name="dialect">org.hibernate.dialect.HSQLDialect</property>
```

Now you are set and you are ready to run your tests using an in-memory database.
