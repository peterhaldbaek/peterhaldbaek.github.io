---
title: Concrete - My very own Android ORM
layout: post
tags: ["java","android"]
date: 2010-10-28
commentsUrl: http://www.briskbee.com/2010/10/concrete-my-very-own-android-orm.html#comment-form
redirect_from: "/posts/concrete_-_my_very_own_android_orm/"
---

I finally decided to write my own ORM for the Android platform simply because I thought this was really needed. The only option I found was the [Active Android](http://www.activeandroid.com) framework which is not open source in any way. So I figured it was time to code my own framework. So here is [Concrete](http://github.com/peterhaldbaek/concrete)! The framework is still very rudimentary to say the least but if I find the time I think it will work out pretty nicely. I have been influenced by Hibernate, the Active Record pattern and of course Active Android. In the current state it does not support relations (yikes!) but I am working on this so stay tuned.

So how does the framework work? It is pretty easy. Let's say you have a table called <code>Foo</code> with the string <code>myText</code>, the integer <code>myNumber</code> and the date <code>myDate</code>. All you have to do to get basic CRUD operations on this table is to create this object:

```
package com.briskbee.concrete.example.record;

import java.util.Date;
import android.content.Context;
import com.briskbee.concrete.Record;

public class Foo extends Record<Foo> {
  public String myText;
  public int myNumber;
  public Date myDate;

  public Foo(Context context) {
    super(context);
  }
}
```

Please note all variables of the class are public. This is necessary for the framework to be able to recognize them as properties of the class (and columns of the table). So now we have an object let's use it. Let's say we are inside an activity and want to create a row in the table foo.

```
Foo foo = new Foo(this);
foo.myText = "some text";
foo.myNumber = 3;
foo.myDate = new Date();
foo.save();
```

That is basically all there is to it. No need for writing tedious CRUD code anymore. It is as DRY as it gets. Retrieving, updating and deleting records are just as easy:

```
// Load foo
Foo foo = new Foo(this);
foo.load(1);

// Update foo
foo.myText = "some other text";
foo.update();

// Delete foo
foo.delete();
```

That is pretty neat in my book. The framework relies heavily on reflection and I have no clue how this performs. The code runs faster if you are writing all the hardwiring by hand but it is also pretty boring and very un-DRY.

As a developer you still need to have your own implementation of <code>SQLiteOpenHelper</code> which creates the tables for you. I am planning to implement some automatic stuff for this too but fixing relations is a higher priority at the moment.

## Update 08.01.2014
Active Android is now open source so my little framework has become somewhat obsolete.