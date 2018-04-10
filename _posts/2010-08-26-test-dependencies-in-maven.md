---
title: Test dependencies in Maven
layout: post
tags: ["java","test","maven"]
date: 2010-08-26
commentsUrl: http://www.briskbee.com/2010/08/test-dependencies-in-maven.html#comment-form
redirect_from: "/posts/test_dependencies_in_maven/"
---

For some time I have struggled with test dependencies between modules in Maven. Imagine this situation:

*   Module A contains a base test class
*   Module B depends on module A
*   Module B imports the base test class of module A in its unit test

This will create a <code>NoClassDefFoundError</code> and I have spent numerous hours (days, months?) trying to figure out what the problem was. Until today when I came across this helpful [post](http://www.mailinglistarchive.com/users@maven.apache.org/msg33474.html). If you are too lazy to follow the link I will give you the solution here. You need to let module A generate the test classes in a test jar file and make module B dependent on this jar file. Add this to the pom.xml of module A to make it generate the test jar file:

```
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-jar-plugin</artifactId>
  <executions>
    <execution>
      <goals>
       <goal>test-jar</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```

And add this to the pom.xml of module B to make it dependent on it:

```
<dependency>
  <groupId>some.group</groupId>
  <artifactId>A</artifactId>
  <version>1.0</version>
  <type>test-jar</type>
  <scope>test</scope>
</dependency>
```

Build module A by issuing the command <code>mvn clean install</code> and run your tests in module B by issuing the command <code>mvn clean test</code>. That should do the trick.
