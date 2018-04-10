---
title: Hibernate, Oracle, HSQLDB and sequences
layout: post
tags: ["java","hibernate","hsqldb","test","oracle"]
date: 2011-09-22
commentsUrl: http://www.briskbee.com/2011/09/hibernate-oracle-hsqldb-and-sequences.html#comment-form
redirect_from: "/posts/hibernate__oracle__hsqldb_and_sequences/"
---

Recently I was writing some unit tests where I needed to access a database sequence for generation of unique numbers. The production environment uses Oracle while my unit tests use [HQSLDB](http://hsqldb.org/). The numbers from the sequence are extracted by ordinary SQL through [Hibernate](http://www.hibernate.org/) meaning that I could not rely on specifying the database dialect since the SQL is hardcoded. The code used for extracting the numbers (in Oracle) looked something like this


```
long number =
((BigDecimal) sessionFactory.getCurrentSession().createSQLQuery
  ("select my_seq.nextval from dual")
  .uniqueResult()
).longValue();
```
Running this in a unit test results in a ClassCastException since HSQLDB either generates the primitive int or a BigInteger instance depending on how the sequence was created. A simple solution for this is to add a scalar to the query determining the type of the returned number.


```
long number =
((BigDecimal) sessionFactory.getCurrentSession().createSQLQuery
  ("select my_seq.nextval as id from dual")
  .addScalar("id", new BigDecimalType())
  .uniqueResult()
).longValue();
```
Voila! The number is now converted to a BigDecimal by Hibernate and the ClassCastExceptions are long gone.
