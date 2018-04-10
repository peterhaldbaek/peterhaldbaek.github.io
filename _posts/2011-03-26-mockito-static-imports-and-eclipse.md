---
title: Mockito, static imports and Eclipse
layout: post
tags: ["java","eclipse","test","mockito"]
date: 2011-03-26
commentsUrl: http://www.briskbee.com/2011/03/mockito-static-imports-and-eclipse.html#comment-form
redirect_from: "/posts/mockito__static_imports_and_eclipse/"
---

I recently started using [Mockito](http://mockito.org/) as a mocking framework. Mockito makes heavy use of static imports which by default in Eclipse are not accessible through code completion. It is possible to use the Mockito methods by accessing them through their class by writing <code>Mockito.when</code> but this would quickly clutter your code with a lot of Mockito references. Code completion has been around for ages so not having it feels like coding in Notepad or vi. Fortunately I have found out that it is actually possible and quite easy to configure Eclipse to add the static imports which is a great relief.

Simply open the Preferences in Eclipse and go to the menu item Java -> Editor -> Content Assist -> Favorites and press the button New Type.... Enter the text <code>org.mockito.Mockito</code> and press OK. Now you have all static methods on the Mockito class available to you through code completion.

![Eclipse screenshot](/assets/img/screen_2bshot_2b2011-03-26_2bat_2b2_17_02_2bpm.png)

Other useful Mockito classes to add are <code>org.mockito.Matchers</code> and <code>org.mockito.BDDMockito</code> (if you are into [BDD](http://en.wikipedia.org/wiki/Behavior_Driven_Development)).
