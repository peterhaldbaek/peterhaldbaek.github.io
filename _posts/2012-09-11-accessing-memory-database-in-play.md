---
layout: post
title: "Accessing memory database in Play!"
tags: java h2 play database
date: 2012-09-11
commentsUrl: http://www.briskbee.com/2012/09/accessing-memory-databse-in-play.html#comment-form
redirect_from: "/posts/accessing_memory_database_in_play_/"
---

The other day I was running some unit tests in an app using the [Play! framework](http://www.playframework.org/) and I wanted to see how the data in the database looked like. Tests are run using a H2 memory database so accessing this was not as straightforward as I am used to since most configuration in Play! is done by convention (or magic, not sure which). So I did not know the connection string to the database nor the credentials used by the framework. After some [googling](http://stackoverflow.com/questions/6265957/access-mem-or-fs-database-tables-using-h2-console) I came across the information needed.

First start your test and when you are at a breakpoint open the database browser supplied by Play! at localhost:9000/@db (this was new to me as well). Select the Generic Server option and use these parameters.

| Parameter | Value            |
|-----------|------------------|
| JDBC URL  | jdbc:h2:mem:play |
| User Name | sa               |
| Password  | &lt;none&gt;     |

You should now be able to access the database used by your tests.
